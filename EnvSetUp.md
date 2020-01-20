## Table of Contents
#### _Follow this list of instructions in order to setup the following in Ubuntu_ : 
   0. [Ubuntu](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-ubuntu)
   1. [Java](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-java)
   2. [Maven](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-maven)
   3. [Tomcat](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-tomcat)
   4. [Spring Boot CLI](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-spring)
   5. [Git](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-git)
   6. [Docker](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-docker)
   7. [Postgres](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-postgres)
   8. [Keycloak](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-keycloak)
   9. [VSCode](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-VSCode)
   10. [Node](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-Node)
   11. [SDKMan](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-SDKMan)
   12. [Tor](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-tor)
   13. [DBeaver](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#installing-DBeaver)
  
### Note : Each package will require editing environment variables

## Installing Ubuntu
1. Download Ubuntu ISO : https://www.ubuntu.com/download/desktop
2. Create Bootable USB : https://rufus.ie/
3. https://vitux.com/how-to-install-ubuntu-18-04-along-with-windows-10/

Replace Windows entirely :  
1. Plug in Ubuntu USB
2. Restart computer, hit F12 (or F10) as soon as it restarts to get to the BIOS menu
3. Navigate to something named "BOOT" to find a boot sub-menu
4. Select Hard Disk Drives > 1st Boot Device > Select USB
5. Follow Ubuntu Instructions : https://www.avoiderrors.com/boot-computer-using-usb-drive/

Launch terminal by pressing `Ctrl+Alt+T`    

#### Uninstall Ubuntu Software
1. Open the terminal
2. Type `dpkg --list` and hit Enter / Return
3. Find the name of the program to delete
4. `sudo apt-get --purge remove name_of_program`
5. Enter root password / Confirm with "Y"

## Installing Java 
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#table-of-contents) 

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
### Verify the install
```console
java -version  #Java 
javac -version #JDK / compiler
```
### Find Java
```console
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
### Java Environment Variables 
```console 
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
```console
# Define PATH again with the route to gedit / nano, then edit it 
export PATH="$PATH:/usr/bin"  
sudo nano /etc/environment  

# Refresh the file when done with "source /path/to/file_that_was_updated" : 
source /etc/environment  
```

## Installing Maven 
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#table-of-contents)

[Download](https://maven.apache.org/download.cgi)  
[Install](https://maven.apache.org/install.html)  
[How to Install on Linux](https://www.javahelps.com/2017/10/install-apache-maven-on-linux.html)  
[Install Maven on Ubuntu](https://tecadmin.net/install-apache-maven-on-ubuntu/)  
[Maven Environment Variables](https://askubuntu.com/questions/275704/how-to-permanently-set-environmental-variables-path-and-m2-home-in-ubuntu-for-ma)  
1. Download Binary or Source file. Source == human-readable and needs to be compiled first
2. Unzip the file in the preferred location
3. Edit PATH variable to the `./bin` directory. E.g. `"C:\Program Files\Java\apache-maven-3.6.1\bin"`


### Install with Apt
```console
# Install the latest version
sudo apt update  
sudo apt install maven  
mvn -version  
```
### Download from official website
```console
# Navigate to the directory where you want to install
cd /usr/local  

# Specify the version you want (optional syntax, can hardcode if preferred)
export VER="3.6.1"  

# Get, Unpack, then Rename the File
wget http://www-eu.apache.org/dist/maven/maven-3/${VER}/binaries/apache-maven-${VER}-bin.tar.gz  
sudo tar -xzf apache-maven-${VER}-bin.tar.gz  
sudo ln -s apache-maven-${VER} apache-maven  

# Copy the extracted directory to the /opt/directory : 
cp -r apache-maven-${VER} /opt/maven  
```
### Environment Variables 
```console
# Method 1  
export JAVA_HOME=/usr/lib/jvm/default-java  
export M2_HOME=/opt/maven  
export MAVEN_HOME=/opt/maven  
export PATH=${M2_HOME}/bin:${PATH}  

# Method 2
sudo nano /etc/environment  
# add to PATH =":${M2_HOME}/bin"  
# add M2_HOME="/usr/share/maven"  
# add MAVEN_HOME="/usr/share/maven"  

# Verify 
mvn -version  
```

### Where is Maven?  
```console
# Find the executable   
whereis mvn  
# Find libs and repo  
locate maven  
# Locate command, then find a particular library  
locate maven | grep 'jetty'  

ls -lsa /usr/share/maven  
ls -lsa /etc/maven  
```

### Building an Executable JAR
```console
# Run the App in the directory with the pom.xml file
./mvnw spring-boot:run

# Or, build the JAR file then run it
./mvnw clean package
java -jar target/gs-securing-web-0.1.0.jar

# Go to http://localhost:8080
```

## Installing Tomcat
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#table-of-contents)

[Tomcat](http://tomcat.apache.org/)  
[Install Tomcat on Ubuntu](https://www.javahelps.com/2015/03/install-apache-tomcat-on-ubuntu.html)  
[Install Tomcat8 on Ubuntu16.04](https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-ubuntu-16-04)  

#### Download Tomcat 
[Download Tomcat](https://tomcat.apache.org/download-80.cgi)  
**Note : Tomcat should not be run under the root user. Create a new system user named "tomcat" and group with home directory /opt/tomcat that will run the Tomcat service :**  
```console
sudo useradd -r -m -U -d /opt/tomcat -s /bin/false tomcat  

# Change the directory 
cd /opt/  

# Extract Tomcat 
sudo tar -xvzf ~/Downloads/apache-tomcat-9.0.24-fulldocs.tar.gz  
sudo tar -xvzf ~/Downloads/apache-tomcat-9.0.24.tar.gz  

# (Optional) Rename the folder to 'apache-tomcat' 
/opt$ ls --> tomcat-9.0-doc  
sudo mv tomcat-9.0-doc/ apache-tomcat-doc/  
/opt$ ls --> apache-tomcat-9.0.24  
sudo mv apache-tomcat-9.0.24/ apache-tomcat/

# (Optional) start Tomcat without root privilege
sudo chmod -R 777 apache-tomcat/  

# Edit the environment variables 
sudo gedit /etc/environment  
# Add this to the end of the line 
CATALINA_HOME="/opt/apache-tomcat"  
source /etc/environment  

## Windows example for CATALINA_HOME : "C:\Program Files\Java\apache-tomcat-9.0.22"

# Start the Tomcat Server :
$CATALINA_HOME/bin/startup.sh  
# Or
catalina start  
# Go to the following URL :
http://localhost:8080  

# View available commands
catalina -h

# Stop the Server : 
$CATALINA_HOME/bin/shutdown.sh  
```
#### Download Tomcat from the Terminal
```console
wget http://www-us.apache.org/dist/tomcat/tomcat-9/v9.0.24/bin/apache-tomcat-9.0.24.tar.gz  
tar xzf apache-tomcat-9.0.24.tar.gz  
sudo mv apache-tomcat-9.0.24 /usr/local/apache-tomcat9  
sudo rm apache-tomcat-9.0.24.tar.gz  

# Make the .sh files executable
chmod +x ./bin/startup.sh  
chmod +x ./bin/shutdown.sh  

# Add this to the end of the file : CATALINA_HOME="/usr/local/apache-tomcat9"  
sudo gedit /etc/environment 

# Start Tomcat
cd /usr/local/apache-tomcat9/bin  
./startup.sh  

# GOTO : http://localhost:8080  
./shutdown.sh  
```

## Installing SPRING
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#table-of-contents)

[1) Install Spring on Ubuntu](https://jeromejaglale.com/doc/spring4_tutorial/installation_ubuntu)  
[2) Install Spring on Ubuntu](https://stackoverflow.com/questions/49889906/how-do-i-install-spring-boot-cli-in-ubuntu)  
[3) Install Spring on Ubuntu](https://howtoprogram.xyz/2016/08/28/install-spring-boot-command-line-interface-on-linux/)  
[Spring Boot from Command Line](http://appsdeveloperblog.com/run-spring-boot-app-from-a-command-line/)  
[Deploy Spring with NGINX](https://www.linode.com/docs/development/java/how-to-deploy-spring-boot-applications-nginx-ubuntu-16-04/)  

```console
# Check if it's installed
spring --version  
# Remove existing files
sudo apt-get remove spring  
# remove dependent packages
sudo apt-get remove --auto-remove spring  
# also delete config files 
sudo apt-get purge spring  
sudo apt-get purge --auto-remove spring  

# Download, Unpack and Rename
wget https://repo.spring.io/release/org/springframework/boot/spring-boot-cli/2.1.7.RELEASE/spring-boot-cli-2.1.7.RELEASE-bin.tar.gz  
tar xzf spring-boot-cli-2.1.7.RELEASE-bin.tar.gz  
sudo mv spring-2.1.7.RELEASE /usr/local/spring  

# Add SPRING CLI to PATH
sudo gedit /etc/environment
# Add \spring-boot-cli-{$VERSION}.RELEASE\bin to PATH
# E.g. : \usr\local\spring-boot-cli-1.2.3.RELEASE\bin
# Verify :
spring --version
spring --help

# Run Spring Boot Groovy scripts :
spring run <SpringBoot-Groovy-Scriptname>
# https://www.journaldev.com/8195/spring-boot-cli-setup-and-helloworld-example
spring run HelloWorld.groovy

## Start the embedded shell :
spring shell
version
run [SCRIPT_NAME].groovy


# Go to a desired project directory
cd /home/joseph/Desktop
# Initialize the project
spring init --build=maven --dependencies=web --name=hello hello-world

Approximate Output Structure:
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


# Build the app
# From the directory with the pom.xml file (i.e. 'hello-world' here)
# This will create a /target directory with "hello-world-0.0.1-SNAPSHOT.jar" ready to run
mvn install _OR_ maven package 

# Run Spring Boot with Jar command :
java -jar target/hello-world-0.0.1-SNAPSHOT.jar  

# Run Spring Boot using Maven :
mvn spring-boot:run  

# GOTO : http://localhost:8080
```
#### Spring Boot CLI on Windows
1. Download Spring Boot CLI : https://repo.spring.io/release/org/springframework/boot/spring-boot-cli/
2. Add to Path : C:\Spring\spring-2.1.7.RELEASE\bin
3. SPRING_HOME : C:\Spring\spring-2.1.7.RELEASE

## Installing Git
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#table-of-contents)

[Git Website](https://git-scm.com/)  

#### Installing Git w/ Default Packages
```console
sudo apt update  
sudo apt install git-all  
```

#### For Ubuntu, this PPA provides the latest stable upstream Git version
```console
sudo add-apt-repository ppa:git-core/ppa  
sudo apt update  
sudo apt install git  

# Check version
git --version 

# Clone a repo
git clone username@hostname:/path/to/repository  
```
## Setting-Up Git Repos 
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvironmentSetUp.md#table-of-contents)  

```console
git config --global user.name
git config --global user.name "Firstname Lastname"

git config --global user.email
git config --global user.email "email@address.com"
```
1. [Generate new SSH Key and Add it to the ssh-agent](https://help.github.com/en/enterprise/2.15/user/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
2. [Add a new ssh key to your GitHub acct](https://help.github.com/en/enterprise/2.15/user/articles/adding-a-new-ssh-key-to-your-github-account)

1. Open Git Bash
2. Paste this text into the terminal :
```console
ssh-keygen -t rsa -b 4096 -C "your-emailaddress-in-github"
```
```console
user@ID5CD9088ZN2 MINGW64 ~
$ ssh-keygen -t rsa -b 4096 -C "your-emailaddress-in-github"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/user/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/user/.ssh/id_rsa.
Your public key has been saved in /c/Users/user/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:randomnumbersandletters your-emailaddress-in-github
The key's randomart image is:
+---[RSA 4096]----+
| .    =+*o=.     |
|o .  + * + .     |
| *  . *   o E    |
|+ .  o o . .     |
|+.  . . S        |
|+o.. * o         |
|oo..+ *          |
|*..+ = o         |
|**. o +          |
+----[SHA256]-----+

user@ID5CD9088ZN2 MINGW64 ~
$ eval $(ssh-agent -s)
Agent pid 11584

user@ID5CD9088ZN2 MINGW64 ~
$ ssh-add ~/.ssh/id_rsa
Identity added: /c/Users/user/.ssh/id_rsa (your-emailaddress-in-github)

user@ID5CD9088ZN2 MINGW64 ~
$ clip < ~/.ssh/id_rsa.pub
```

#### [get and copy new key to GitHub](https://virtualzero.net/blog/how-to-add-an-ssh-public-key-to-github-from-Ubuntu-18.04-lts)
```console
ssh-keygen -t rsa -b 4096 -C "your-emailaddress-in-github"

### Can leave blank when prompted: 
Enter file in which to save the key (/home/joehanlon/.ssh/id_rsa):          
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 

### Output:  
The key fingerprint is:
SHA256:randomnumbersandletters your-emailaddress-in-github
The key's randomart image is:
+---[RSA 4096]----+
|  E..o.+ =.=+    |
| . *. o.* =. + o |
|. =.+  +.   . = .|
|.+.+ o ... .   o |
|o.+ + . S +    ..|
|.  + .     o ...o|
|    o       . ..o|
|   . .       .+  |
|    .       .o . |
+----[SHA256]-----+

### Output the new key : 
cat ~/.ssh/id_rsa.pub

### Copy the contents and paste to GitHub
### Save the new key

### Make sure that you use SSH to clone the repository, or else you may have to use your username + password every time you use git
### Related : https://stackoverflow.com/questions/7773181/git-keeps-prompting-me-for-a-password
```

```console
C:\path\to\project>git clone git@github.stuff.com:owner_or_groupname/repo_name.git
```
#### Git Cheatsheets : 
[Git Cheatsheet 1](https://www.atlassian.com/dam/jcr:8132028b-024f-4b6b-953e-e68fcce0c5fa/atlassian-git-cheatsheet.pdf)  
[Git Cheatsheet 2](https://education.github.com/git-cheat-sheet-education.pdf)  
[Git Cheatsheet 3](https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf)  
[Git Cheatsheet 4](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet)  

## Installing Docker
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#table-of-contents)  

[Install Docker on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04)  
[Install Docker-CE Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)  
### Uninstall old versions 
```console
sudo apt-get remove docker docker-engine docker.io  
```
### Install Docker  
```console
sudo apt update  
sudo apt install apt-transport ca-certificates curl software-properties-common  
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"  
sudo apt update  
apt-cache policy docker-ce  
sudo apt install docker-ce  
sudo systemctl status docker  
```

### Install Docker from the Official Repository  
```console
sudo apt-get update  
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common  
```
To clarify, here’s a brief breakdown of each command:
  - **apt-transport-https** : Allows the package manager to transfer files and data over https
  - **ca-certificates** : Allows the system (and web browser) to check security certificates
  - **curl** : This is a tool for transferring data
  - **software-properties-common** : Adds scripts for managing software
```console
# Add Dockers GPG Key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add –  
# Install the Docker Repo
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs)  stable"  
```
The command **$(lsb_release –cs)** scans and returns the codename of your Ubuntu installation (bionic).  
Also, the final word of the command (stable) is the type of Docker release.

```console
sudo apt-get update  
# Install latest
sudo apt-get install docker-ce 
# (Optional) Pick a version method 1
apt-cache madison docker-ce  
# (Optional) Pick a version method 2
sudo apt-get install docker-ce=<VERSION>  
```

## Installing Postgres 
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#table-of-contents)  

[Install PostreSQL on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04)  
[Install PostgreSQL Server on Ubuntu](https://tecadmin.net/install-postgresql-server-on-ubuntu/)  
[Install pgAdmin4 on Ubuntu](https://tecadmin.net/install-pgadmin4-on-ubuntu/)  
[Create and Manage Tables](https://www.digitalocean.com/community/tutorials/how-to-create-remove-manage-tables-in-postgresql-on-a-cloud-server)  
[Remove Postgres](https://www.liquidweb.com/kb/how-to-remove-postgresql/)  
```console
sudo apt update  
sudo apt install postgresql postgresql-contrib  
# Switch over to the postgres account on your server
sudo -i -u postgres 
# Access the Postgres through the CLI psql
psql 

# Default pw for postgres is nothing, so set it with : 
ALTER USER postgres PASSWORD 'newpassword';  
# Moved to : 
postgres=#   
# Exit out of the PostgreSQL prompt by typing :
postgres=# \q  
# Access Postgres w/o Switchin Accounts
sudo -u postgres psql  

# Create a New Role
# Note : --interactive will prompt you for the name and permissions
postgres@server:~$ createuser --interactive 
# --OR--  
sudo -u postgres createuser --interactive  

# Output : 
Enter name of role to add: sammy  
Shall the new role be a superuser? (y/n) y  
# Check out options by looking at the man page 
man createuser  

# Create a New Database 
postgres@server:~$ createdb sammy  
# --OR--
sudo -u postgres createdb sammy  

# Open Postgres Prompt with New Role
sudo adduser sammy  
sudo -i -u sammy  
psql  

# --OR-- 
sudo -u sammy psql  

# If you want your user to connect to a different database, specify the new one like this :
psql -d postgres  
# Once logged in, check the current connection info by typing : 
sammy=# \conninfo  

# Create a Table (Syntax)
CREATE TABLE table_name (  
    column_name1 col_type (field_length) column_constraints,  
    column_name2 col_type (field_length),  
    column_name3 col_type (field_length)  
);  

# Mock Table
CREATE TABLE playground (  
    equip_id serial PRIMARY KEY,  
    type varchar (50) NOT NULL,  
    color varchar (25) NOT NULL,  
    location varchar(25) check (location in ('north', 'south', 'west', 'east', 'northeast', 'southeast', 'southwest', 'northwest')),  
    install_date date  
);  

# View the new table :
sammy=# \d  

# View the table w/o the sequence :
sammy=# \dt  

# Insert Data into the Table
sammy=# INSERT INTO playground (type, color, location, install_date) VALUES ('slide', 'blue', 'south', '2017-04-28');  
sammy=# INSERT INTO playground (type, color, location, install_date) VALUES ('swing', 'yellow', 'northwest', '2018-08-16');  
COMMIT;  

# Select 
sammy=# SELECT * FROM playground;  
# Delete 
sammy=# DELETE FROM playground WHERE type = 'slide';  
# Alter by adding / dropping a column
sammy=# ALTER TABLE playground ADD last_maint date;   
sammy=# ALTER TABLE playground DROP last_maint;  
# Update Data in a Table 
sammy=# UPDATE playground SET color = 'red' WHERE type = 'swing';  

# Connect to Postgres
sudo su - postgres  
psql  

# Check login info 
postgres-# \conninfo  
postgres-# \q  

# Install PgAdmin4
# Use servers IP address or domain name followed by /pgAdmin4 as subdirectory URL.
sudo apt-get install pgadmin4 pgadmin4-apache2  
```
### Remove Postgres
```console
# List the postgres packages 
dpkg -l | grep postgres  
# Delete the Postgres packages 
apt-get --purge remove command  
sudo apt-get --purge remove _(list of packages like : pgdg-keyring postgresql-10 postgresql-client-10 postgresql-client-common postgresql-common)_  
# Verifying the Deletion of Postgres
root@newclient:~# dpkg -l | grep postgres  
root@newclient:~#  

# Delete the database and directories at the same time 
rm -rf /usr/local/pgsql  
# Remove the postgres user account 
userdel postgres  

# Simplest way is :
sudo apt-get --purge remove postgresql  
# --OR, delete it in parts--  
dpkg -l | grep postgres  
sudo apt-get --purge remove _postgresql postgresql-doc postgresql-common ..._  
sudo deluser postgres  

# Completely remove postgres in terminal 
sudo apt-get --purge remove postgresql\*  
# --OR--  
sudo apt-get --purge remove postgresql*  

# Confirm 
whereis postgres  
whereis postgresql  

# Misc.
psql -V  
locate bin/psql  
=# SELECT version();  
=# SHOW server_version;  

# Get back to Ubuntu root user 
\q to quit out of postgres  
```

## Installing KeyCloak 
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#table-of-contents)

[Install Keycloak on Ubuntu 18.04](https://medium.com/@hasnat.saeed/setup-keycloak-server-on-ubuntu-18-04-ed8c7c79a2d9)  
[Download Keycloak](https://www.keycloak.org/downloads.html)  
[Keycloak : Getting Started](https://www.keycloak.org/docs/latest/getting_started/index.html?source=post_page-----ed8c7c79a2d9----------------------)  
[Keycloak : REST API](https://www.keycloak.org/docs-api/6.0/rest-api/index.html?source=post_page-----ed8c7c79a2d9----------------------)  
[Keycloak : Server Installation](https://www.keycloak.org/docs/6.0/server_installation/?source=post_page-----ed8c7c79a2d9----------------------)  
[Quick Guide to Using Keycloak](https://www.comakeit.com/quick-guide-using-keycloak-identity-access-management/)  
[Spring Boot & Keycloak w/ OIDC](https://www.baeldung.com/spring-boot-keycloak "Baeldung")  
[Spring Boot & Keycloak](https://medium.com/@techgeek628/easily-secure-your-spring-boot-applications-with-keycloak-41e09acc88fd?source=post_page-----ed8c7c79a2d9----------------------)  
[Intro to Keycloak](http://www.mastertheboss.com/jboss-frameworks/keycloak/introduction-to-keycloak "JBoss")  
[Start & Stop Wildfly](https://bgasparotto.com/start-stop-restart-wildfly/)  

```console
java -version  
sudo apt-get update  
sudo apt-get install default-jdk -y  
java -version  

cd /usr/local  
sudo wget https://downloads.jboss.org/keycloak/7.0.0/keycloak-7.0.0.tar.gz   _(This will be a LONG download. ~30min)_  
sudo tar -xvzf keycloak-7.0.0.tar.gz  
sudo mv keycloak-7.0.0 /usr/local/keycloak  
```

We should not run Keycloak under the root user for security reasons.  
Let’s create a group keycloak and add a user keycloak to it.  

```console
sudo groupadd keycloak  
sudo useradd -r -g keycloak -d /usr/local/keycloak -s /sbin/nologin keycloak  

# Modify the ownership and permission of the /keycloak directory
sudo chown -R keycloak: keycloak  

# Give executable permissions to /keycloak/bin directory
sudo chmod o+x /usr/local/keycloak/bin/  
cd /etc/  

# Create a configuration directory for Keycloak under the /etc directory
sudo mkdir keycloak  
# Copy KeyCloak config file
sudo cp /usr/local/keycloak/docs/contrib/scripts/systemd/wildfly.conf /etc/keycloak/keycloak.conf  
# Copy the launch script
sudo cp /usr/local/keycloak/docs/contrib/scripts/systemd/launch.sh /usr/local/keycloak/bin/ 
# Make keyloack user as the owner of this script so it can execute it
sudo chown keycloak: /usr/local/keycloak/bin/launch.sh  

# Update the installation path in the launch.sh file :
# Change WILDFLY_HOME to the root directory of keycloak from /opt/wildfly --> /usr/local/keycloak
sudo nano /usr/local/keycloak/bin/launch.sh
# --OR--  
sudo gedit /usr/local/keycloak/bin/launch.sh  

# Copy service definition file 
sudo cp /usr/local/keycloak/docs/contrib/scripts/systemd/wildfly.service /etc/systemd/system/keycloak.service  

#### Edit the service file
sudo gedit /etc/systemd/system/keycloak.service  
```

```console
[Unit]  
Description=The Wildfly Application Server --> The Keycloak Server
After=syslog.target network.target  
Before=httpd.service

[Service]
Environment=LAUNCH_JBOSS_IN_BACKGROUND=1
EnvironmentFile=-/etc/wildfly/wildfly.conf --> /etc/keycloak/keycloak.conf
User=wildfly --> keycloak
--> Group=keycloak
LimitNOFILE=102642
PIDFile=/var/run/wildfly/wildfly.pid --> /var/run/keycloak/keycloak.pid 
ExecStart=/opt/wildfly/bin/launch.sh $WILDFLY_MODE $WILDFLY_CONFIG $WILDFLY_BIND --> /usr/local/keycloak/bin/launch.sh $WILDFLY_MODE $WILDFLY_CONFIG $WILDFLY_BIND
StandardOutput=null

[Install]
WantedBy=multi-user.target
```

```console
# Reload the systemd manager config and enable keycloak service on system startup
sudo systemctl daemon-reload  
sudo systemctl enable keycloak  

# Start the service
sudo systemctl start keycloak  

# Check the status
sudo systemctl status keycloak  

# Can also tail the Keycloak server logs 
sudo tail -f /usr/local/keycloak/standalone/log/server.log  
```

Now access the Keycloak server at : 
http://<instance-public-ip>:8080/auth/  
e.g. :  http://localhost:8080/auth

Since we are accessing the server from **outside of localhost**,  
we have to use the bash script (add-user-keycloak.sh)  
available under /usr/local/keycloak/bin/ directory  
to create the initial administrator account.  

```console
# Create new master user
sudo /usr/local/keycloak/bin/add-user-keycloak.sh -r master -u <username> -p <password>  

# Restart the Keycloak server
sudo systemctl restart keycloak  

# Navigate to http://<instance-public-ip>:8080/auth/  

# Admin CLI works by making HTTP requests to Admin REST endpoints.  
# Access to them is protected and requires authentication.  
# Admin CLI is located at :  
/usr/local/keycloak/bin/kcadm.sh 

# Login, in order to CRUD
sudo /usr/local/keycloak/bin/kcadm.sh config credentials --server http://localhost:8080/auth --realm master --user <admin-username> –-password <admin-password>  

# Update Realms
sudo /usr/local/keycloak/bin/kcadm.sh update realms/master -s sslRequired=NONE  

# Navigate to : http://<instance-public-ip>:8080/auth/admin/  

# Add an address console to bind to 
# Add this to the end of the file : 
# WILDFLY_MANAGEMENT_CONSOLE_BIND=0.0.0.0
sudo nano /etc/keycloak/keycloak.conf  
# --OR--  
sudo gedit /etc/keycloak.keycloak.conf  


# Open launch.sh in /usr/local/keycloak/bin/ directory and change its contents
sudo gedit /usr/local/keycloak/launch.sh  

if ["x$WILDFLY_HOME" = "X" ]; then
  $WILDFLY_HOME="/usr/local/keycloak"

fi 

if [[ "$1" == "domain" ]]; then
  $WILDFLY_HOME/bin/domain.sh -c $2 -b $3 -bmanagement $4
else 
  $WILDFLY_HOME/bin/standalone.sh -c $2 -b $3 -bmanagement $4
fi 


# Open Keycloak's system service definition file and make the following changes
# Add $WILDFLY_MANAGEMENT_CONSOLE_BIND to ExecStart line (near the bottom)
sudo gedit /etc/systemd/system/keycloak.service  

# Reload and Restart
sudo systemctl daemon-reload  
sudo systemctl restart keycloak  

# Go to http://<instance-public-ip>:9990  

# Create a management user 
sudo /usr/local/keycloak/bin/add-user.sh  
```
When you add a user, you will be taken to an interactive set of questions, like the following : 
```console
- What type of user do you wish to add? a) Management User , b) Application User  
a  
- Enter username  
wf  
- Enter password  
<pw>  
- Are you sure you want to use this password? 
yes  
- What groups do you want to add the user to?  
<left blank>
- Is this correct?  
yes  
```
The output looks like : 
```console
Added user 'wf' with groups to file '/usr/local/keycloak/standalone/configuration/mgmt-users.properties  
Added user 'wf' with groups to file '/usr/local/keycloak/domain/configuration/mgmt-users.properties  

- Is this new user going to be used for one AS process to connect to another AS process?  
yes  
```
```
# Restart Keycloak
sudo systemctl restart keycloak  

# Now refresh the browser, which will now prompt for the WildFly credentials 
# Now we have setup a Keycloak server and enable / configured remote access to admin and management console
```

## Installing VSCode
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#table-of-contents)  

[Install VSCode on Ubuntu 18.04](https://linuxize.com/post/how-to-install-visual-studio-code-on-ubuntu-18-04/)  
[Best VSCode Extensions for Web Dev](https://scotch.io/bar-talk/22-best-visual-studio-code-extensions-for-web-development)  
```console
# Install the dependencies  
sudo apt update  
sudo apt install software-properties-common apt-transport-https wget  

# Import the Microsoft GPG key 
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -  

# Enable VSCode repo 
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"  

# Install the latest version 
sudo apt update  
sudo apt install code  
```
#### Start VSCode
```console
# In the terminal, anywhere
code 
# In a particular directory (current directory is now workspace)
code . 
```
#### Update VSCode
```console
sudo apt update  
sudo apt upgrade  
```

Pull up extensions : ctrl+shift+x  
Pull up command palette : ctrl+shift+p  

## Installing Node
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvSetUp.md#table-of-contents)  

[How to install Node & npm on Ubuntu 18.04](https://linuxize.com/post/how-to-install-node-js-on-ubuntu-18.04/ "Linuxize")  
[How to install Node on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04 "Digital Ocean")  

*Please note* : Avoid installing Node.js with `apt-get` on Ubuntu. If you have already installed it with the built-in package manager, remove it: 
```console
sudo apt-get purge nodejs && sudo apt-get autoremove && sudo apt-get autoclean
```


#### Install through the Terminal
```console
sudo apt update
sudo apt install nodejs

# (Optional) Update NPM
sudo npm install npm@latest -g

# Verify install
nodejs --version
node -v
npm -v
```
#### Install using PPA
[Install Node on Ubuntu](https://github.com/nodesource/distributions#debinstall "Nodesource")  
```console
# Install with sudo privileges
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```
#### Install using NVM (Node Version Manager)
[NVM Github](https://github.com/nvm-sh/nvm)  
```console
# Download and install the package with either of the following commands 
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
# Or 
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash

# Configure
export NVM_DIR=”$HOME/.nvm”
[ -s “$NVM_DIR/nvm.sh” ] && \. “$NVM_DIR/nvm.sh”
[ -s "$NVM_DIR/bash_completion" ] && \.   "$NVM_DIR/bash_completion"

# Verify the installation
nvm --version
## Or use : 
command -v nvm

# List Nodejs versions installed
nvm list

# List Node.js versions available 
nvm ls-remote

# Install the latest version
nvm install node

# Install a specific version 
nvm install 8.11.1

# Install the latest Long-Term Support version
nvm install --lts

# Switch to using most recently installed version
nvm use 8.11.1

# List multiple Node versions installed 
nvm ls

# Make a version your default type 
# This version will now be automatically used
nvm alias default 8.11.1

# It can also be referenced as "default"
nvm use default

# Specify which version of Node to run (if multiple installed)
nvm run node v9.3.0
```
#### Upgrade Node
Best thing to do would actually be to remove node and re-install.  
1. open terminal (crtl+alt+t).
2. type which node, will give path something like `/usr/local/bin/node`
4. remove node with `rm -rf node`
5. make sure node doesn't show up with `node -v`
6. download it again with `curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -` or with `install sudo apt-get install -y nodejs`

#### Remove Node
[Install & Uninstall Node 1](https://ircama.github.io/osm-carto-tutorials/nodejs-commands/)  
[Install & Uninstall Node 2](https://dxtright.com/index.php/2018/09/20/install-nvm-node-js-globally-linux-based-system/)  

```console
sudo apt remove npm
sudo apt remove nodejs
sudo apt purge nodejs
sudo apt autoremove

## Or 
sudo apt-get purge --auto-remove nodejs

## Or
sudo apt-get remove nodejs <br/>
sudo apt-get remove npm <br/>
sudo apt-get update <br/>
which node

# To uninstall a version you have enabled using nvm 
nvm current
nvm uninstall node_version # e.g. nvm uninstall v9.3.0
nvm deactivate
```
#### (Optional) Install Build Tools
```console
# To compile and install native addons from npm you may also need to install build tools
sudo apt-get install -y build-essential
# Or use : 
sudo apt install build-essential
```
#### Install & Create a New React Project
```console
# 2 Steps
npm install -g create-react-app
create-react-app my_project

# 1 Step
npx create-react-app my_project
```
#### Run the React App
```console
cd my_project
npm start
# Go to http://localhost:3000
```

## Installing SDKMan
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvironmentSetUp.md#table-of-contents)  

[Install SDKMan](https://sdkman.io/install)  
[SDKMan GitHub](https://github.com/sdkman/sdkman-cli)  

```console
curl -s "https://get.sdkman.io" | bash

# Open new terminal then enter :
source "$HOME/.sdkman/bin/sdkman-init.sh"

# Verify installation succeeded :
sdk version
```

## [Installing Tor](https://linuxize.com/post/how-to-install-tor-browser-on-ubuntu-18-04/)  
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvironmentSetUp.md#table-of-contents)  

```
sudo apt update
sudo apt install apt-transport-https
sudo add-apt-repository universe

wget -q -O - https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | sudo apt-key add -

echo "deb https://deb.torproject.org/torproject.org $(lsb_release -cs) main" | sudo tee -a /etc/apt/sources.list

sudo apt update
sudo apt install tor deb.torproject.org-keyring torbrowser-launcher
```

#### Update Tor Browser
```
sudo apt update
sudo apt upgrade
```

#### Uninstall Tor
```
sudo apt-get remove --purge tor
sudo apt-get remove --purge torbrowser-launcher
```

## [Installing DBeaver](https://computingforgeeks.com/install-and-configure-dbeaver-on-ubuntu-debian/)  
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvironmentSetUp.md#table-of-contents)  

[Install DBeaver on 18.04](https://www.osradar.com/install-dbeaver-ubuntu-18-04-linux-mint-19/)  
```
## Upgrade the system
sudo apt update && sudo apt upgrade

## Install default Java development runtime 
sudo apt install default-jdk

## Find the GPG Key
wget -O - https://dbeaver.io/debs/dbeaver.gpg.key | sudo apt-key add -

## Add the repo
echo "deb https://dbeaver.io/debs/dbeaver-ce /" | sudo tee /etc/apt/sources.list.d/dbeaver.list

## Install DBeaver
sudo apt update && sudo apt install dbeaver-ce
```

## Installing Yarn
```console
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - 
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list 
sudo apt update 
sudo apt install yarn 
sudo apt install --no-install-recommends yarn 
yarn --version 
```
#### Create a new project with Yarn
```console
yarn init my_yarn_project <br/>
yarn add package_name <br/>
yarn add package_name@version_or_tag
yarn upgrade package_name
yarn upgrade package_name@version_or_tag
yarn remove package_name
```

#### Install all project dependencies that are specified in package.json
```console
yarn
## Or use :
yarn install 
```

## Links
[ToC](https://github.com/joehanlon/Linux/blob/master/EnvironmentSetUp.md#table-of-contents)  
## A
## B 
## C
#### Cassandra
[Apache Cassandra](http://cassandra.apache.org/download/)  
#### Commons
[Apache Commons](http://commons.apache.org/)  
#### CouchDB
[Apache CouchDB](http://couchdb.apache.org/)  
## D
#### Docker 
[Docker](https://www.docker.com/)  
#### Draw.io
[Draw.io](https://www.draw.io/)  
## E
## F
## G
#### GIMP
[GIMP](https://www.gimp.org/downloads/)  
#### Git
[Download Git](https://git-scm.com/download/)  
#### Gradle
[Install Gradle](https://www.tutorialspoint.com/gradle/gradle_installation.htm)  
### GraphQL 
#### GraphQL-Java
[GraphQL Java](https://www.graphql-java.com/documentation/v12/)  
## H
#### HTTP Server
[Apache HTTP Server](http://httpd.apache.org/download.cgi)  
## I
#### Inkscape
[Inkscape](https://inkscape.org/)  
#### IntelliJ
[Install IntelliJ on Ubuntu 18.04 1](https://linuxize.com/post/how-to-install-intellij-idea-on-ubuntu-18-04/)  
[Install IntelliJ on Ubuntu 18.04 2](https://linuxconfig.org/install-intellij-on-ubuntu-18-04-bionic-beaver-linux)  
#### InVision
[InVision](https://www.invisionapp.com/)  
## J
#### Java
[Oracle Java](https://www.oracle.com/technetwork/java/javase/downloads/index.html)  
#### Jenkins
[Jenkins](https://jenkins.io/)  
[Download Jenkins Windows Installer](https://jenkins.io/download/thank-you-downloading-windows-installer-stable)  
#### Jupyter Notebook
[Install Jupyter Notebook](https://jupyter.org/install)  
## K
#### Kafka 
NB: Depends on Apache Zookeeper to run  
[Download Kafka](https://kafka.apache.org/downloads)  
#### Keycloak
[Keycloak Documentation](https://github.com/keycloak/keycloak-documentation/blob/master/securing_apps/topics/oidc/java/spring-security-adapter.adoc)  
[Keycloak Downloads](https://www.keycloak.org/downloads.html)  
[Keycloak SAML Tomcat Adapter](https://www.keycloak.org/docs/5.0/securing_apps/index.html#_saml-tomcat-adapter)  
#### Kubernetes
[Kubernetes](https://kubernetes.io/)  
## L
#### Linux Permissions
[Linux Permissions](https://www.cyberciti.biz/faq/unix-linux-bsd-chmod-numeric-permissions-notation-command/)  
## M
#### Mattermost (Alt. to Slack)
[Mattermost](https://mattermost.com/download/)  
#### MongoDB
[Install MongoDB on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/ "Official")  
[Install MongoDB on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-18-04)  
#### MySQL  
[MySQL](https://dev.mysql.com/downloads/)  
[Install MySQL on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04)  
## N  
#### Neo4j
[Neo4J](https://debian.neo4j.org/)  
[Install Neo4J on Ubuntu](https://neo4j.com/docs/operations-manual/current/installation/linux/debian/ "Official")  
[Install Neo4J on Ubuntu](https://linuxhint.com/install-neo4j-ubuntu/)  
#### NGINX  
[NGINX](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)  
[Install NGINX on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04)  
#### Nifi
[Download Nifi](https://nifi.apache.org/download.html)  
## O
## P
#### Postman
[Postman](https://www.getpostman.com/)  
[Install Postman on Ubuntu 18.04 1](https://linuxize.com/post/how-to-install-postman-on-ubuntu-18-04/)  
[Install Postman on Ubuntu 18.04 2](http://ubuntuhandbook.org/index.php/2018/09/install-postman-app-easily-via-snap-in-ubuntu-18-04/)  
#### Personal Package Archive
[PPA](https://itsfoss.com/ppa-guide/ "Personal Package Archive")  
#### Primia
[Prisma.io](https://www.prisma.io/docs/1.1/faq/pricing-fae2ooth2u)  
#### Python
[Python](https://www.python.org/downloads/)  
## Q
## R
#### R
[R](https://cran.r-project.org/)  
[R Studio](https://www.rstudio.com/products/rstudio/)  
## S
#### Spark
[Download Apache Spark](http://spark.apache.org/downloads.html)  
#### Spring
[Spring](https://spring.io)  
[Spring Initializr](https://start.spring.io/)  
[Spring Security](https://spring.io/projects/spring-security)  
[Spring Security SAML](https://projects.spring.io/spring-security-saml/)  
[Spring Security SAML Reference](https://docs.spring.io/spring-security-saml/docs/current/reference/htmlsingle/)  
[Spring Security SAML API](https://docs.spring.io/spring-security-saml/docs/current/api/)  
[Spring Security SAML GitHub](https://github.com/spring-projects/spring-security-saml)  
[Spring Security Extension RELEASES : Spring Security SAML](https://repo.spring.io/list/release/org/springframework/security/extensions/spring-security-saml/)  
[Spring Boot CLI](https://repo.spring.io/release/org/springframework/boot/spring-boot-cli/)  
## T
#### Tomcat
[Tomcat](http://tomcat.apache.org/)  
#### Tor
[Install Tor on Ubuntu](https://itsfoss.com/install-tar-browser-linux/#comments)  
## U
#### Ubuntu
[Install Ubuntu Alongside Windows 10](https://vitux.com/how-to-install-ubuntu-18-04-along-with-windows-10/)  
1. Download Ubuntu ISO : https://www.ubuntu.com/download/desktop
2. Create Bootable USB : https://rufus.ie/
3. https://vitux.com/how-to-install-ubuntu-18-04-along-with-windows-10/
## V
#### Visual Studio Code
[VSCode](https://code.visualstudio.com/download)  
## W
## X
## Y
#### Yarn
[Yarn](https://yarnpkg.com/lang/en/)  
## Z
#### Zookeeper
[Download Apache Zookeeper](https://zookeeper.apache.org/releases.html#download)  
## # 
[7-zip](https://www.7-zip.org/)  
## Misc.
[Good Presentation Java Web Security](https://events.static.linuxfound.org/sites/events/files/slides/anatomy-secure-web-app-acna-2017-v2.pdf)  
[Sample SAML App](https://github.com/Endeios/samlv2app)  
