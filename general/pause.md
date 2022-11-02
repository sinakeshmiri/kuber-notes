<h1> pause container </h1>

the pause container serves as the "parent container" for all of the containers in your pod. 

  - core responsibilities:
  
    - basis of Linux namespace sharing
    
    - PID 1 for each pod and reaps zombie processes
    
  unshare namespaces in  linux for chid procs:
  
 `sudo unshare --pid --uts --ipc --mount -f chroot rootfs /bin/sh`
 
 
 https://www.ianlewis.org/en/almighty-pause-container
