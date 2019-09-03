java --version  
sudo apt update  
sudo apt-get update  
sudo apt-get install default-jdk  
  
sudo apt-get install software-properties-common  
sudo add-apt-repository ppa:linuxuprising/java  
sudo apt-get install oracle-java11-installer  
  
sudo update-alternatives --config java  
## Enter the version number (in the Selection column) then press Enter  
## Edit the environment file   
sudo nano /etc/environment  
## Add JAVA_HOME at the end of the file  
JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"  
Nano : Ctrl+X --> Exit (prompted to save changes); Ctrl+O should work too. Overwrite the name if needed.  
## verify!  
echo $JAVA_HOME  
  
