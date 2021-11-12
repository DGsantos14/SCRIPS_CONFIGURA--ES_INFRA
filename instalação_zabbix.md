# Configurações iniciais 
yum install -y net-tools vim nano epel-release wget curl tcpdump traceroute telnet dnf python3 htop

# Editando Arquivo de configuração do selinux
vim /etc/selinux/config
'Altera' SELINX=enforce
	 SELINUX=disabled
sestatus
setenforce 0

# Instalando repositorio Zabbix
rpm -Uvh https://repo.zabbix.com/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.el7.noarch.rpm
yum clean all

# Instalando repositorio mysql
rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm

# Instalando mysql 8
yum install -y mysql-community-server

# Ativando o mysql
systemctl enable --now mysqld
systemctl status mysqld

# Buscando a senha do MySql 
 cat /var/log/mysqld.log
 grep 'generated for' /var/log/mysqld.log

# Criando o banco de dados para o zabbix

mysql -u root -p

create database zabbix character set utf8 collate utf8_bin;

create database zabbix character set utf8 collate utf8_bin;

create user zabbix_web@localhost identified with mysql_native_password by '@Zabbix5';
create user zabbix_srv@localhost identified with mysql_native_password by '@Zabbix5';

grant all privileges on zabbix.* to zabbix_srv@localhost;
grant select, update, delete, insert on zabbix.* to 'zabbix_web'@'localhost';

flush privileges;

exit

# Instalando o ZABBIX
yum install -y zabbix-server-mysql

# Popular o banco de dados do zabbix com o schema do mysql:
zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uzabbix_srv -pzabbix zabbix

# Editar o arquivo zabbix_server.conf
vi /etc/zabbix/zabbix_server.conf

