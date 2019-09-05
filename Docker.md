
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04  

#### Install Docker  
sudo apt update  
sudo apt install apt-transport ca-certificates curl software-properties-common  
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"  
sudo apt update  
apt-cache policy docker-ce  
sudo apt install docker-ce  
sudo systemctl status docker  

#### Run Docker Command without Sudo (Optional)
sudo usermod -aG docker ${USER}  
su - ${USER}  
id -nG  

sudo usermod -aG docker username  

#### Using the Docker Command 
docker [option] [command] [arguments]  
docker run hello-world  
docker search ubuntu  
docker pull ubuntu  
docker images  

#### Run 
docker run -it ubuntu  _(combination of -i and -t)_  
Output _(Shows you are now in the container d9b100f2f636)_  
root@d9b100f2f636:/#  
root@d9b100f2f636:/# apt update  
root@d9b100f2f636:/# apt install nodejs  
root@d9b100f2f636:/# node -v  

#### Managing Docker Containers
docker ps  
docker ps -a  
docker ps -l  
docker start d9b100f2f636  
docker stop sharp_volhard  
docker rm festive_williams  
docker commit -m "What you did to the image" -a "Author Name" container_id repository/new_image_name  
docker commit -m "added Node.js" -a "sammy" d9b100f2f636 sammy/ubuntu-nodejs  
docker login -u docker-registry-username  
docker push docker-registry-username/docker-image-name  
docker push sammy/ubuntu-nodejs  
docker pull sammy/ubuntu-nodejs  

#### Notes 
You can start a new container and give it a name using the --name switch.  
You can also use the --rm switch to create a container that removes itself when it’s stopped.  
The -m switch is for the commit message that helps you and others know what changes you made,  
while -a is used to specify the author.  

## 2019 Tutorial for Docker & Ubuntu 18.04  
https://phoenixnap.com/kb/how-to-install-docker-on-ubuntu-18-04  
sudo apt-get update  _(get the latest revisions to local database)_  
sudp apt-get remove docker docker-engine docker.io  _(remove old docker software)_  
sudo apt install docker.io  
sudo systemctl start docker  
sudo systemctl enable docker  
docker --version _(check the version)_  

### Install Docker form the Official Repository  
sudo apt-get update  
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common  
```
To clarify, here’s a brief breakdown of each command:

apt-transport-https: Allows the package manager to transfer files and data over https
ca-certificates: Allows the system (and web browser) to check security certificates
curl: This is a tool for transferring data
software-properties-common: Adds scripts for managing software
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add –  _(Add Dockers GPG Key)_  
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs)  stable"  _(Install the Docker Repo )_  
_(The command “$(lsb_release –cs)” scans and returns the codename of your Ubuntu installation – in this case, Bionic.  
Also, the final word of the command – stable– is the type of Docker release.)_  

```
sudo apt-get update  
sudo apt-get install docker-ce  _(install latest)_
apt-cache madison docker-ce  _(pick a version)_  
sudo apt-get install docker-ce=<VERSION>  _(install a specific version)_  
```
#### Install from a .deb Package (Optional)
https://download.docker.com/linux/ubuntu/dists/bionic/  
sudo dpkg -i /path/to/package.deb  

https://docs.docker.com/install/linux/docker-ce/ubuntu/  
#### Uninstall old versions 
sudo apt-get remove docker docker-engine docker.io  
sudo apt-get update  
sudo apt-get install \  
  apt-transprot-https \  
  ca-certificates \  
  curl \  
  gnupg-agent \  
  software-properties-common  
  
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  

#### Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.
sudo apt-key fingerprint 0EBFCD88  

sudo add-apt-repository \  
   "deb [arch=arm64] https://download.docker.com/linux/ubuntu \  
   $(lsb_release -cs) \  
   stable"  
sudo apt-get update  
sudo apt-get install docker-ce docker-ce-cli containerd.io  _(get the latest version)_  

