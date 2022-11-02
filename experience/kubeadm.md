important points:
  - your calico CIDR in config file and kubeadm must be the same :
        
        --pod-network-cidr + custom-resources.yaml
    
  - your cluster cidr must be a subset of you cni cidr (/etc/cni/net.d/100-crio-bridge.conf)
  
  - you can use this commands to  bypass sanctions or filtering
  ```
  no_proxy=10.0.0.0/8,192.168.0.0/16,127.0.0.1,172.16.0.0/16 https_proxy=socks5://x.x.x.x:1080 tsocks  kubeadm init  --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers
  ```


source :
https://computingforgeeks.com/deploy-kubernetes-cluster-on-ubuntu-with-kubeadm/
