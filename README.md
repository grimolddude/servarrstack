# servarrstack


Some basic rundown of what the heck is going on:

THE PATH TO WEBSERVER

Configure NAS as normal
TrueNAS scale - 
	add cluster
	create NFS mount point

****

All clients:

apt update
apt upgrade 
								
[ Unprivileged NFS read/write from NAS to LXC container ]
Proxmox Host node:

create mountpoint:

#mkdir /mnt/pve/TITANshare

check nfs access

#apt install nfs-common

add to fstab

10.0.0.133:/mnt/TITAN/Titan_Media /mnt/pve/TITANshare nfs defaults 0 0

reload node 

#systemctl daemon-reload

#mount -a
	
	--> At this point, read write access is available from NAS share to ProxMox Host Node
	
	Create mount point inside container (ONLY REQUIRED FOR MEDIA ENGINE)
	#mkdir /mnt/TITANshare
	add hardpoint to shell
	#nano /etc/pve/lxc/101.conf  		(where 101.conf is container id)
		mp0: /mnt/pve/TITANshare,mp=/mnt/TITANshare
		{ source mount point , target mount directory }
	
	
Get template requirements;

In this instance, using DC Racetracks

#apt update && apt upgrade -y && apt install git ansible -y

#git clone https://github.com/grimolddude/proxmoxbase.git

adjust scripts: 00-bootstrap.sh 

edit line 14, 15, 27

sh 00-bootstrap.sh 

set user account

change password
#

run script:

 ansible-playbook 03-docker-directories.yml && sh 04-install-docker.sh && sh 05-install-docker-containers.sh && ansible-playbook 06-install-tools.yml && ansible-playbook 08-create-dirs.yml

Navigate IP map:

Portainer-agent - LXC_IP:9001

NodeExporter - LXC_IP::9100

target work directory;

git clone https://github.com/grimolddude/docker.git

Core deploy complete ------------------ > Clone into required sub containers

Remember: Change each LXC container IP address through Host Node UI

Housekeeping: add each container to Portainer to make sure Watchtower works properly

		> From here, deploy each service per container
		
Stack order is probably important
First attempt in this order:

	Portainer		10.0.0.131/24 : 9443

	jackett			10.0.0.132/24 : 9117
	prowlarr		10.0.0.121/24 : 9118
	flaresolverr	10.0.0.134/24 : 9119

	sonarr			10.0.0.135/24 : 7878
	radarr			10.0.0.136/24 : 8989
	requestrr		10.0.0.137/24 : 6767
	notifiarr		10.0.0.138/24 : 5656
	overseerr		10.0.0.139/24 : 9898
	prometheus		10.0.0.140/24 : 9999
	qtorrent		10.0.0.141/24 : 8899



	adguard			10.0.0.142/24 : 9797


	grafana			10.0.0.143/24 : 9988
	homarr			10.0.0.144/24 : 7788
	influxdb		10.0.0.145/24 : 6677




Docker Container stack - use per target


	  
	________________________________________________
	
