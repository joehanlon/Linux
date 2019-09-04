Launch terminal by pressing Ctrl+Alt+T  

java --version  
sudo apt update  
sudo apt-get update  
sudo apt-get install default-jdk  
  
sudo apt-get install software-properties-common  
sudo add-apt-repository ppa:linuxuprising/java  
sudo apt-get install oracle-java11-installer  
  
sudo update-alternatives --config java  
## Enter the version number (in the Selection column) then press Enter  
  
## Find location of Java 
### Method 1
readlink -f $(which java)  
### Method 2
whereis java  --> "/usr/bin/java  ..."  
ls -l /usr/bin/java  --> "/etc/alternatives/java"  
ls -l /etc/alternatives/java  --> "/usr/lib/jvm/java-11-openjdk-amd64/bin/java"  

## Environment Variables !!!
## Edit the environment file   
sudo nano /etc/environment  
## Add JAVA_HOME at the end of the file  
JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"  
Nano : Ctrl+X --> Exit (prompted to save changes); Ctrl+O should work too. Overwrite the name if needed.  
## verify!  
echo $JAVA_HOME  
## If nothing shows up, set JAVA_HOME to where /bin/java is located :
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64/  
### or
sudo nano /etc/environment  
### add this line in the environment file : 
JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"  

## You can install multiple JRE / JDK at a time. 
## You need to select the number on the far left to choose one
sudo update-alternatives --config java   
## Similarly you can configure your JDK :
sudo update-alternatives --config javac     

## If PATH gets messed up : 
export PATH="$PATH:/usr/bin"  
sudo nano /etc/environment  
### Refresh the file when done : 
source /etc/environment  
source /path/to/file_that_was_updated  

## Edit PATH 
add ":$JAVA_HOME/bin" to the end of the PATH string  

## Maven 
https://maven.apache.org/download.cgi  
https://maven.apache.org/install.html  
https://www.javahelps.com/2017/10/install-apache-maven-on-linux.html  
https://tecadmin.net/install-apache-maven-on-ubuntu/  
https://askubuntu.com/questions/275704/how-to-permanently-set-environmental-variables-path-and-m2-home-in-ubuntu-for-ma  

Needs java JDK and JAVA_HOME already
### Install with Apt
sudo apt update  
sudo apt install maven  
mvn -version  
### Latest Release of Apache Maven 
#### Download from official website
cd /usr/local  
export VER="3.6.1"  
wget http://www-eu.apache.org/dist/maven/maven-3/${VER}/binaries/apache-maven-${VER}-bin.tar.gz  
sudo tar -xzf apache-maven-${VER}-bin.tar.gz  
sudo ln -s apache-maven-${VER} apache-maven  
#### Copy the extracted directory to the /opt/directory : 
cp -r apache-maven-${VER} /opt/maven  

### Environment Variables 
#### Method 1  
export JAVA_HOME=/usr/lib/jvm/default-java  
export M2_HOME=/opt/maven  
export MAVEN_HOME=/opt/maven  
export PATH=${M2_HOME}/bin:${PATH}  
#### Method 2
sudo nano /etc/environment  
add to PATH =":${M2_HOME}/bin"  
add M2_HOME="/usr/share/maven"  
add MAVEN_HOME="/usr/share/maven"  

### Verify 
mvn -version  
### Where is Maven?  
#### Find the executable   
whereis mvn  
#### Find libs and repo  
locate maven  
#### Locate command, then find a particular library  
locate maven | grep 'jetty'  

ls -lsa /usr/share/maven  
ls -lsa /etc/maven  


## Tomcat
https://www.javahelps.com/2015/03/install-apache-tomcat-on-ubuntu.html  
### Tomcat should not be run under the root user. Create a new system user named "tomcat" and group with home directory /opt/tomcat that will run the Tomcat service :
sudo useradd -r -m -U -d /opt/tomcat -s /bin/false tomcat  
#### Download Tomcat 
https://tomcat.apache.org/download-80.cgi  
#### Change the directory 
cd /opt/  
#### Extract Tomcat 
sudo tar -xvzf ~/Downloads/apache-tomcat-9.0.24-fulldocs.tar.gz  
#### (Optional) Rename the folder to 'apache-tomcat' 
/opt$ ls --> tomcat-9.0-doc  
sudo mv tomcat-9.0-doc/ apache-tomcat/  
#### (Optional) start Tomcat without root privilege
sudo chmod -R 777 apache-tomcat/  
#### Edit the environment variables 
sudo gedit /etc/environment  
#### Add this to the end of the line 
CATALINA_HOME="/opt/apache-tomcat"  
source /etc/environment  
#### Start the Tomcat Server :
$CATALINA_HOME/bin/startup.sh  
#### Go to the following URL :
http://localhost:8080  
#### Stop the Server : 
$CATALINA_HOME/bin/shutdown.sh  
