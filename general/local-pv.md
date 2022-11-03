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


source: https://vocon-it.com/2018/12/20/kubernetes-local-persistent-volumes/
