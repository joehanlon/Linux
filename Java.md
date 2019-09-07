## Follow this list of instructions in order to setup the following in Ubuntu : 
   1. Java
   2. Maven
   3. Tomcat
   4. Spring Boot CLI
### Note : Each package will require editing environment variables

Launch terminal by pressing Ctrl+Alt+T  
### Installing Java 
```console
# Check if java is installed, if not, install the latest default package
java --version  
sudo apt update  
sudo apt-get update  
sudo apt-get install default-jdk  

# Manually install a particular version by updating the PPA
sudo apt-get install software-properties-common  
sudo add-apt-repository ppa:linuxuprising/java  
sudo apt-get install oracle-java11-installer  

# Choose a version of Java if you have multiple by selecting the number then press Enter
sudo update-alternatives --config java  

# Similarly you can configure your JDK :
sudo update-alternatives --config javac   
```
### Find Java
```
# Method 1
readlink -f $(which java)

# Method 2 
$ whereis java 
Output: 
/usr/bin/java ...

$ ls -l /usr/bin/java 
Output:
/etc/alternatives/java

$ ls -l /etc/alternatives/java
Output :
"/usr/lib/jvm/java-11-openjdk-amd64/bin/java" 
```
[PPA](https://itsfoss.com/ppa-guide/ "Personal Package Archive")

### Java Environment Variables 
```
# Use either of the following to open the environment file
sudo nano /etc/environment 
sudo gedit /etc/environment

# Add JAVA_HOME at the end of the file
# This should be where /bin/java is located
JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"

# Save and Exit Nano or Gedit
# Verify it worked :
echo $JAVA_HOME  

# Edit PATH 
add ":$JAVA_HOME/bin" to the end of the PATH string  
```

### If PATH gets messed up in the process : 
```
# Define PATH again with the route to gedit / nano, then edit it 
export PATH="$PATH:/usr/bin"  
sudo nano /etc/environment  

# Refresh the file when done with "source /path/to/file_that_was_updated" : 
source /etc/environment  
```

### Installing Maven 
[Download](https://maven.apache.org/download.cgi)  
[Install](https://maven.apache.org/install.html)  
[How to Install on Linux](https://www.javahelps.com/2017/10/install-apache-maven-on-linux.html)  
[Install Maven on Ubuntu](https://tecadmin.net/install-apache-maven-on-ubuntu/)  
[Maven Environment Variables](https://askubuntu.com/questions/275704/how-to-permanently-set-environmental-variables-path-and-m2-home-in-ubuntu-for-ma)  

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
https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-ubuntu-16-04  
### Tomcat should not be run under the root user. Create a new system user named "tomcat" and group with home directory /opt/tomcat that will run the Tomcat service :
sudo useradd -r -m -U -d /opt/tomcat -s /bin/false tomcat  
#### Download Tomcat 
https://tomcat.apache.org/download-80.cgi  
#### Change the directory 
cd /opt/  
#### Extract Tomcat 
sudo tar -xvzf ~/Downloads/apache-tomcat-9.0.24-fulldocs.tar.gz  
sudo tar -xvzf ~/Downloads/apache-tomcat-9.0.24.tar.gz  
#### (Optional) Rename the folder to 'apache-tomcat' 
/opt$ ls --> tomcat-9.0-doc  
sudo mv tomcat-9.0-doc/ apache-tomcat-doc/  
/opt$ ls --> apache-tomcat-9.0.24  
sudo mv apache-tomcat-9.0.24/ apache-tomcat/
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

wget http://www-us.apache.org/dist/tomcat/tomcat-9/v9.0.24/bin/apache-tomcat-9.0.24.tar.gz  
tar xzf apache-tomcat-9.0.24.tar.gz  
sudo mv apache-tomcat-9.0.24 /usr/local/apache-tomcat9  
sudo rm apache-tomcat-9.0.24.tar.gz  
chmod +x ./bin/startup.sh  
chmod +x ./bin/shutdown.sh  
sudo gedit /etc/environment --> CATALINA_HOME="/usr/local/apache-tomcat9"  

cd /usr/local/apache-tomcat9  
chmod +x ./bin/startup.sh  
chmod +x ./bin/shutdown.sh  
cd /usr/local/apache-tomcat9/bin  
./startup.sh  
GOTO : http://localhost:8080  
./shutdown.sh  

https://www.cyberciti.biz/faq/unix-linux-bsd-chmod-numeric-permissions-notation-command/  

## SPRING
sudo add-apt-repository ppa:spring  
sudo apt-get update  
sudo apt-get install spring  
spring --version  
sudo apt-get remove spring  
#### remove dependent packages
sudo apt-get remvoe --auto-remove spring  
#### also delete config files 
sudo apt-get purge spring  
sudo apt-get purge --auto-remove spring  
https://jeromejaglale.com/doc/spring4_tutorial/installation_ubuntu  

wget https://repo.spring.io/release/org/springframework/boot/spring-boot-cli/2.1.7.RELEASE/spring-boot-cli-2.1.7.RELEASE-bin.tar.gz  
tar xzf spring-boot-cli-2.1.7.RELEASE-bin.tar.gz  
sudo mv spring-2.1.7.RELEASE /usr/local/spring  

https://howtoprogram.xyz/2016/08/28/install-spring-boot-command-line-interface-on-linux/
https://stackoverflow.com/questions/49889906/how-do-i-install-spring-boot-cli-in-ubuntu
https://www.linode.com/docs/development/java/how-to-deploy-spring-boot-applications-nginx-ubuntu-16-04/
http://appsdeveloperblog.com/run-spring-boot-app-from-a-command-line/
cd /home/joseph/Desktop
spring init --build=maven --dependencies=web --name=hello hello-world
### ^^ This creates a folder named "hello-world" with the following structure :
hello-world  
|_ HELP.md  
|_ mvnw  
|_ mvnw.cmd  
|_ pom.xml  
|_ src  
   |_ main  
   |  |_ java  
   |  |  |_ com  
   |  |     |_ example  
   |  |        |_ helloworld  
   |  |           |_ HelloApplication.java 
   |  |_ resources  
   |     |_ application.properties  
   |     |_ static  
   |     |_ templates  
   |_ test  

## Build the app
#### From the directory with the pom.xml file 
mvn install _OR_ maven package 

#### This will create a /target directory with "hello-world-0.0.1-SNAPSHOT.jar" ready to run
#### Run Spring Boot with Jar command :
java -jar target/hello-world-0.0.1-SNAPSHOT.jar  

#### Run Spring Boot using Maven :
mvn spring-boot:run  
