**How conatiners are isolated?**
----

namespaces + cgroups


**namespaces :** *provides a fake view for proccess*
  - hostname
  -  pid
  - fs
  -  network interfaces
  - ipc

**CGroups:**  *cgroup limits the resources that the process can use*
  - CPU *limited by milli-cores*
  - RAM *limited by bytes of RAM*
  - block I/O
  - network I/O
  - ...
  
 
 ` Namespaces and cgroups also work on groups of processes. `
 
 *You can use same namespace for other containers as well*
 ```
 docker run -d --name nginx -v `pwd`/nginx.conf:/etc/nginx/nginx.conf -p 8080:80 nginx
 docker run -d --name ghost  --net=container:nginx --ipc=container:nginx --pid=container:nginx ghost
 ```
`--net --ipc --pid`

![kub1](https://user-images.githubusercontent.com/72389059/199221946-9a048493-a871-4484-872e-5089ff312fca.png)

----


**kuber  pods?**

----

![pods](https://user-images.githubusercontent.com/72389059/199222298-5a5047dd-d758-4f4d-b8a6-953f23148f9b.png)

*feels like it's running on the same machine. They can talk to each other on localhost, they can use shared volumes. They can even use IPC or send each other signals like HUP or TERM*

`By being able to combine containers into Pods this way we can essentially create containers that can be added to Pods as an "API" that others can consume`


