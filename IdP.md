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
sudo useradd -r -g keycloak -d /usr/local/keycloak -s /sbin/nologin keycloak  
sudo chown -R keycloak: keycloak  _(modify the ownership and permission of the /keycloak directory)_  
sudo chmod o+x /usr/local/keycloak/bin/  _(give executable permissions to /keycloak/bin directory)_  
cd /etc/  
sudo mkdir keycloak  _(create a configuration directory for Keycloak under the /etc directory)_  
sudo cp /usr/local/keycloak/docs/contrib/scripts/systemd/wildfly.conf /etc/keycloak/keycloak.conf  _(Copy KeyCloak config file)_
sudo cp /usr/local/keycloak/docs/contrib/scripts/systemd/launch.sh /usr/local/keycloak/bin/ _(copy the launch script)_  
sudo chown keycloak: /usr/local/keycloak/bin/launch.sh  _(make keyloack user as the owner of this script so it can execute it)_  

#### Update the installation path in the launch.sh file :
sudo nano /usr/local/keycloak/bin/launch.sh
--OR--  
sudo gedit /usr/local/keycloak/bin/launch.sh  
change WILDFLY_HOME to the root directory of keycloak from /opt/wildfly --> /usr/local/keycloak

#### Copy service definition file 
sudo cp /usr/local/keycloak/docs/contrib/scripts/systemd/wildfly.service /etc/systemd/system/keycloak.service  

#### Edit the service file
sudo gedit /etc/systemd/system/keycloak.service  

```
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

#### Reload the systemd manager config and enable keycloak service on system startup
sudo systemctl daemon-reload  
sudo systemctl enable keycloak  

#### Start the service
sudo systemctl start keycloak  

#### Check the status
sudo systemctl status keycloak  

#### Can also tail the Keycloak server logs 
sudo tail -f /usr/local/keycloak/standalone/log/server.log  

#### Now access the Keycloak server at : 
```
http://<instance-public-ip>:8080/auth/  
e.g. : 
http://localhost:8080/auth
```
Since we are accessing the server from outside of localhost,  
we have to use the bash script (add-user-keycloak.sh)  
available under /usr/local/keycloak/bin/ directory  
to create the initial administrator account.  

sudo /usr/local/keycloak/bin/add-user-keycloak.sh -r master -u <username> -p <password>  
  
sudo systemctl restart keycloak  
navigate to http://<instance-public-ip>:8080/auth/  
