disable  and  stop  the  service

remove /etc/resolv.conf and create  a  new  one ( dont  edit  it , it's a link)

change  config at  `/var/lib/kubelet/config.yaml` by   setting `resolvConf: /run/systemd/resolve/resolv.conf`  to `/etc/resolv.conf`

restart the cluster
