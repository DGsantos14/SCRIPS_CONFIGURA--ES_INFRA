# Instalação 
# AUTOR: -DGSANTOSFKZ-
# Instalação do phipam no Cent-OS 7
# Configurações iniciais: 
yum install -y net-tools vim nano epel-release wget curl tcpdump traceroute telnet dnf python3 htop

## Instalação do phpipam em docker no CENTOS

sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce docker-ce-cli containerd.io

sudo systemctl enable --now docker 
sudo systemctl status docker 

sudo docker run hello-world

mkdir phpipam

docker run --name phpipam-mysql -e MYSQL_ROOT_PASSWORD=@Zabbix5 -v /root/phpipam:/var/lib/mysql -d mysql:5.6

docker run -ti -d -p 8080:80 -e MYSQL_ENV_MYSQL_ROOT_PASSWORD=@Zabbix5 --name ipam --link phpipam-mysql:mysql pierrecdn/phpipam