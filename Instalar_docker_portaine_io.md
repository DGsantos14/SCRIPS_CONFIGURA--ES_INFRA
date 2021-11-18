 # entrar como Administrador
 su -

# Atualizar lista de atualizações
sudo apt-get update 

# Instalar as atualizações

sudo apt-get update 

# Instalar os aplicativos necessários para instalar Docker
 
apt-get install apt-transport-https 
apt-get install ca-certificates 
apt-get install curl 
apt-get install gnupg-agent 
apt-get install  software-properties-common 

# Adicionar chave GPG do Docker
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -  

# Verificar se você tem a chave GPG
sudo apt-key fingerprint 0EBFCD88 

# Fazer o download do script de instalação do Docker
curl -fsSL https://get.docker.com -o get-docker.sh 

# Instalar script de instalação do Docker
sudo sh get-docker.sh 

# Rodar o Docker container "hello-word" para verificar se o Docker foi instalado com sucesso
sudo docker run hello-world 

# Todas essas instruções foram coletadas no site do Docker. Confira o link:

https://docs.docker.com/engine/instal/debian/

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
# Nós iremos utilizar a imagem portainer/portainer. Para instalar o Portainer, confira o link:

https://github.com/portainer/portainer

# Para a instalação do Portainer, foi utilizado os seguintes códigos:

# Criar volume "portainer_data"
docker volume create portainer_data 

# Instalar o Portainer
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer -H unix:///var/run/docker.sock 