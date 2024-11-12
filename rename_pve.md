# Procedure for rename PVE name 
Create backup for original config:
```
mkdir -p /tmp/qemu ## make temp dir for moving VM config files
cp /etc/pve/nodes/$original_hostname/qemu-server/* /tmp/qemu/
```
Change the desired hostname:
```
hostnamectl set-hostname "$new_hostname"
sed -i "s/$original_hostname/$new_hostname/g" /etc/hosts
```
Reload services:
```
services=("pveproxy.service" "pvebanner.service" "pve-cluster.service" "pvestatd.service" "pvedaemon.service")
for service in "${services[@]}"
do
systemctl restart "$service"
done
```
Remove old configuration and copy files to new pve setup:
```
rm -rf "/etc/pve/nodes/$original_hostname"
cp /tmp/qemu/* /etc/pve/nodes/$new_hostname/qemu-server/
rm /tmp/qemu/*
```
