## KeyCloak 
https://medium.com/@hasnat.saeed/setup-keycloak-server-on-ubuntu-18-04-ed8c7c79a2d9  
https://www.keycloak.org/downloads.html 

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
```

sudo groupadd keycloak  
sudo useradd -r -g keycloak -d /keycloak -s /sbin/nologin keycloak  
sudo chown -R keycloak: keycloak  _(modify the ownership and permission of the /keycloak directory)_  
sudo chmod o+x /keycloak/bin/  _(give executable permissions to /keycloak/bin directory)_  
cd /etc/  
sudo mkdir keycloak  _(create a configuration directory for Keycloak under the /etc directory)_  
sudo cp /keycloak/docs/contrib/scripts/systemd/wildfly.conf /etc/keycloak/keycloak.conf  _(Copy KeyCloak config file)_
sudo cp /keycloak/docs/contrib/scripts/systemd/launch.sh /keycloak/bin/ _(copy the launch script)_  
sudo chown keycloak: /keycloak/bin/launch.sh  _(make keyloack user as the owner of this script so it can execute it)_  
