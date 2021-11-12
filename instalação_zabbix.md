# AUTOR: DGSANTOSFKZ
# Instalação do Zabbix_5 no Cent-OS 7
# Configurações iniciais: 
yum install -y net-tools vim nano epel-release wget curl tcpdump traceroute telnet dnf python3 htop

# Editando Arquivo de configuração do selinux:
vim /etc/selinux/config
'Altera' SELINX=enforce
	 SELINUX=disabled
sestatus
setenforce 0

# Instalando repositorio Zabbix:
rpm -Uvh https://repo.zabbix.com/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.el7.noarch.rpm
yum clean all

# Instalando repositorio mysql:
rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm

# Instalando mysql 8:
yum install -y mysql-community-server

# Ativando o mysql:
systemctl enable --now mysqld
systemctl status mysqld

# Buscando a senha do MySql:
 cat /var/log/mysqld.log
 grep 'generated for' /var/log/mysqld.log

# Criando o banco de dados para o zabbix:

mysql -u root -p

create database zabbix character set utf8 collate utf8_bin;

create database zabbix character set utf8 collate utf8_bin;

create user zabbix_web@localhost identified with mysql_native_password by '@Zabbix5';
create user zabbix_srv@localhost identified with mysql_native_password by '@Zabbix5';

grant all privileges on zabbix.* to zabbix_srv@localhost;
grant select, update, delete, insert on zabbix.* to 'zabbix_web'@'localhost';

flush privileges;

exit

# Instalando o ZABBIX:
yum install -y zabbix-server-mysql

# Popular o banco de dados do zabbix com o schema do mysql:
zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uzabbix_srv -pzabbix zabbix

# Editar o arquivo zabbix_server.conf:
vim /etc/zabbix/zabbix_server.conf


# Editar os parametros:
DBUser=zabbix_srv
DBPassword=zabbix (OU A SENHA SUPER SEGURA DE VOCÊS)

# Iniciar o zabbix server:
systemctl start zabbix-server

# Adicionar a inicialização do sistema:
systemctl enable zabbix-server

# verificar log do zabbix:
tail -f /var/log/zabbix/zabbix_server.log 

# Instalando a interface e configurando 

yum install centos-release-scl

vim /etc/yum.repos.d/zabbix.repo

[zabbix]
name=Zabbix Official Repository - $basearch
baseurl=http://repo.zabbix.com/zabbix/5.0/rhel/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-ZABBIX-A14FE591

[zabbix-frontend]
name=Zabbix Official Repository frontend - $basearch
baseurl=http://repo.zabbix.com/zabbix/5.0/rhel/7/$basearch/frontend
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-ZABBIX-A14FE591

[zabbix-debuginfo]
name=Zabbix Official Repository debuginfo - $basearch
baseurl=http://repo.zabbix.com/zabbix/5.0/rhel/7/$basearch/debuginfo/
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-ZABBIX-A14FE591
gpgcheck=1

[zabbix-non-supported]
name=Zabbix Official Repository non-supported - $basearch
baseurl=http://repo.zabbix.com/non-supported/rhel/7/$basearch/
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-ZABBIX
gpgcheck=1

# Instalar o frontend do zabbix:
yum install -y zabbix-web-mysql-scl zabbix-nginx-conf-scl

# Acessar:
vim /etc/opt/rh/rh-nginx116/nginx/nginx.conf

# Comentar as linhas:
server {
        #listen       80 default_server;
        #listen       [::]:80 default_server;
        #server_name  _;

# Abrir o arquivo:
vim /etc/opt/rh/rh-nginx116/nginx/conf.d/zabbix.conf

# Modificar as linhas como abaixo:

server {
        listen          80;
        server_name     _;

# Editar o arquivo do php para definir um timezone:
vim /etc/opt/rh/rh-php72/php-fpm.d/zabbix.conf

# modificar a linha de timezone:
listen.acl_users = apache,nginx
php_value[date.timezone] = America/Sao_Paulo

# iniciar o nginx o php fpm:
systemctl start zabbix-server  rh-nginx116-nginx rh-php72-php-fpm 
systemctl enable zabbix-server  rh-nginx116-nginx rh-php72-php-fpm
systemctl status zabbix-server  rh-nginx116-nginx rh-php72-php-fpm

# Liberando Firewall 

 firewall-cmd --permanent --add-port=10050/tcp
 
 firewall-cmd --permanent --add-port=10051/tcp

 firewall-cmd --permanent --add-port=80/tcp

 firewall-cmd --reload



