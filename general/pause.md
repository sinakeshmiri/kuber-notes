<h1> pause container </h1>

the pause container serves as the "parent container" for all of the containers in your pod. 

  - core responsibilities:
  
    - basis of Linux namespace sharing
    
    - PID 1 for each pod and reaps zombie processes
    
  unshare namespaces in  linux for chid procs:
  
 `sudo unshare --pid --uts --ipc --mount -f chroot rootfs /bin/sh`
 
 
 zombie processes occur when:
 
 
  - parent processes don't call the wait syscall after the child process has finished
  - when the parent process dies before the child


*When a process' parent dies before the child, the OS assigns the child process to the "init" process or PID 1. i.e. The init process "adopts" the child process and becomes its parent. This means that now when the child process exits the new parent (init) must call waitto get its exit code or its process table entry remains forever and it becomes a zombie.*



`pause container runs a very simple process that performs no function but essentially sleeps forever 
 It assumes the role of PID 1 and will reap any zombies by calling wait on them `

 
 https://www.ianlewis.org/en/almighty-pause-container
