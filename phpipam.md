# Instalação 

## Instalação do phpipam em docker no CENTOS

sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce docker-ce-cli containerd.io

sudo systemctl enable --now docker 
sudo systemctl satus docker 

sudo docker rum hello-world

mkdir phpipam

docker run --name phpipam-mysql -e MYSQL_ROOT_PASSWORD=@Zabbix5 -v /root/phpipam:/var/lib/mysql -d mysql:5.6

docker run -ti -d -p 80:80 -e MYSQL_ENV_MYSQL_ROOT_PASSWORD=@Zabbix5 --name ipam --link phpipam-mysql:mysql pierrecdn/phpipam