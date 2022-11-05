# Local Persistent Volumes:
 hostPath volumes work well only on single node clusters
 
 
 Local volumes allow to pin-point PODs to a specific node,making sure that a restarting POD always will find the data storage in the state it had left it before the reboot.
 
 # Step0:
 ```
# on the node, where the POD will be located (node1 in our case):
DIRNAME="vol1"
mkdir -p /mnt/disk/$DIRNAME 
chcon -Rt svirt_sandbox_file_t /mnt/disk/$DIRNAME
chmod 777 /mnt/disk/$DIRNAME


```
 
 
 # Step1:
 persistent local volumes requires:
  - binding mode of WaitForFirstConsumer
 
 ```
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: my-local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```

`kubectl create -f storageClass.yaml`

 # Step2:
 
 create local persistent volume with a reference to the storage class 
 ```
 apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-local-pv
spec:
  capacity:
    storage: 500Gi
  accessModes:
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: my-local-storage
  local:
    path: /mnt/disk/vol1
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
          - node1
 ```
        
 `kubectl create -f persistentVolume.yaml`


# Step3:

Create a Persistent Volume Claim
persistent volume must have the volumeBindingMode: WaitForFirstConsumer
 is the only difference between a local volume and a host volume.
the persistent volume claim is not bound to the persistent volume automatically. it will remain "Available" until the first consumer shows up

```
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: my-claim
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: my-local-storage
  resources:
    requests:
      storage: 500Gi
```

`kubectl create -f persistentVolumeClaim.yaml`


# Step4:
he POD just needs to reference the volume claim. The volume claim, in turn, specifies its resource requirements. 

```
apiVersion: v1
kind: Pod
metadata:
  name: www
  labels:
    name: www
spec:
  containers:
  - name: www
    image: nginx:alpine
    ports:
      - containerPort: 80
        name: www
    volumeMounts:
      - name: www-persistent-storage
        mountPath: /usr/share/nginx/html
  volumes:
    - name: www-persistent-storage
      persistentVolumeClaim:
        claimName: my-claim
```

`kubectl create -f http-pod.yaml`


# StepX:

Attach a new POD to the existing local volume

```
piVersion: v1
kind: Pod
metadata:
  name: centos-local-volume
  labels:
    name: centos-local-volume
spec:
  containers:
  - name: centos
    image: centos
    command: ["/bin/sh"]
    args: ["-c", "while true; do cat /data/index.html; sleep 10; done"]
    volumeMounts:
      - name: my-reference-to-the-volume
        mountPath: /data
  volumes:
    - name: my-reference-to-the-volume
      persistentVolumeClaim:
        claimName: my-claim
```



![image](https://user-images.githubusercontent.com/72389059/200104809-04f6ff7f-e4de-4415-ae08-35cc4ba2da8c.png)


source: https://vocon-it.com/2018/12/20/kubernetes-local-persistent-volumes/
