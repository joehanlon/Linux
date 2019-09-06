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

/usr/local/keycloak/bin/kcadm.sh --> Admin CLI  
Admin CLI works by making HTTP requests to Admin REST endpoints.  
Access to them is protected and requires authentication.  

#### Login, in order to CRUD
sudo /usr/local/keycloak/bin/kcadm.sh config credentials --server http://localhost:8080/auth --realm master --user <admin-username> –-password <admin-password>  

sudo /usr/local/keycloak/bin/kcadm.sh update realms/master -s sslRequired=NONE  
http://<instance-public-ip>:8080/auth/admin/  

sudo nano /etc/keycloak/keycloak.conf  
--OR--  
sudo gedit /etc/keycloak.keycloak.conf  

#### Add this to the end of the file 
```
# The address console to bind to 
WILDFLY_MANAGEMENT_CONSOLE_BIND=0.0.0.0
```

#### Open launch.sh in /usr/local/keycloak/bin/ directory and change its contents
sudo gedit /usr/local/keycloak/launch.sh  

```
#!/bin/bash

if ["x$WILDFLY_HOME" = "X" ]; then
  $WILDFLY_HOME="/usr/local/keycloak"

fi 

if [[ "$1" == "domain" ]]; then
  $WILDFLY_HOME/bin/domain.sh -c $2 -b $3 -bmanagement $4
else 
  $WILDFLY_HOME/bin/standalone.sh -c $2 -b $3 -bmanagement $4
fi 
```

#### Open Keycloak's system service definition file and make the following changes
sudo gedit /etc/systemd/system/keycloak.service  
```
add $WILDFLY_MANAGEMENT_CONSOLE_BIND to ExecStart line (near the bottom)
```
sudo systemctl daemon-reload  
sudo systemctl restart keycloak  
Go to http://<instance-public-ip>:9990  

#### Create a management user 
sudo /usr/local/keycloak/bin/add-user.sh  
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
Added user 'wf' with groups to file '/usr/local/keycloak/standalone/configuration/mgmt-users.properties  
Added user 'wf' with groups to file '/usr/local/keycloak/domain/configuration/mgmt-users.properties  
```
- Is this new user going to be used for one AS process to connect to another AS process?  
yes  

#### Restart Keycloak
sudo systemctl restart keycloak  
#### Now refresh the browser, which will now prompt for the WildFly credentials 
## Now we have setup a Keycloak server and enable / configured remote access to admin and management console
https://www.keycloak.org/docs/latest/getting_started/index.html?source=post_page-----ed8c7c79a2d9----------------------  
https://www.keycloak.org/docs/6.0/server_admin/?source=post_page-----ed8c7c79a2d9----------------------  
https://www.keycloak.org/docs-api/6.0/rest-api/index.html?source=post_page-----ed8c7c79a2d9----------------------  
https://www.keycloak.org/docs/6.0/server_installation/?source=post_page-----ed8c7c79a2d9----------------------  
https://www.keycloak.org/downloads.html?source=post_page-----ed8c7c79a2d9----------------------  
https://www.comakeit.com/quick-guide-using-keycloak-identity-access-management/  
https://www.baeldung.com/spring-boot-keycloak  
https://medium.com/@techgeek628/easily-secure-your-spring-boot-applications-with-keycloak-41e09acc88fd?source=post_page-----ed8c7c79a2d9----------------------  
http://www.mastertheboss.com/jboss-frameworks/keycloak/introduction-to-keycloak  


https://bgasparotto.com/start-stop-restart-wildfly/  
https://hiplab.mc.vanderbilt.edu/projects/soempi/jboss_startstop.html  
