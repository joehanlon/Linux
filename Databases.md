https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04  
## Postgres 
sudo apt update  
sudo apt install postgresql postgresql-contrib  
sudo -i -u postgres _(switch over to the **postgres** account on your server)_  
psql _(access a Postgres prompt)_  
#### Default pw for postgres is nothing, so set it with : 
ALTER USER postgres PASSWORD 'newpassword';  
#### Moved to : 
postgres=#   
#### Exit out of the PostgreSQL prompt by typing :
postgres=# \q  
#### Access Postgres w/o Switchin Accounts
sudo -u postgres psql  

#### Create a New Role
postgres@server:~$ createuser --interactive  _(--interactive will prompt you for the name and permissions)_  
--OR--  
sudo -u postgres createuser --interactive  
#### Output : 
Enter name of role to add: sammy  
Shall the new role be a superuser? (y/n) y  
#### Check out options by looking at the **man** page 
man createuser  

#### Create a New Database 
postgres@server:~$ createdb sammy  
--OR--
sudo -u postgres createdb sammy  

#### Open Postgres Prompt with New Role
sudo adduser sammy  
sudo -i -u sammy  
psql  
--OR-- 
sudo -u sammy psql  

#### If you want your user to connect to a different database, specify the new one like this :
psql -d postgres  
#### Once logged in, check the current connection info by typing : 
sammy=# \conninfo  

#### Create a table 
https://www.digitalocean.com/community/tutorials/how-to-create-remove-manage-tables-in-postgresql-on-a-cloud-server  

CREATE TABLE table_name (  
    column_name1 col_type (field_length) column_constraints,  
    column_name2 col_type (field_length),  
    column_name3 col_type (field_length)  
);  

CREATE TABLE playground (  
    equip_id serial PRIMARY KEY,  
    type varchar (50) NOT NULL,  
    color varchar (25) NOT NULL,  
    location varchar(25) check (location in ('north', 'south', 'west', 'east', 'northeast', 'southeast', 'southwest', 'northwest')),  
    install_date date  
);  

#### View the new table :
sammy=# \d  

#### View the table w/o the sequence :
sammy=# \dt  

#### Insert 
sammy=# INSERT INTO playground (type, color, location, install_date) VALUES ('slide', 'blue', 'south', '2017-04-28');  
sammy=# INSERT INTO playground (type, color, location, install_date) VALUES ('swing', 'yellow', 'northwest', '2018-08-16');  

COMMIT;  

#### Select 
sammy=# SELECT * FROM playground;  
#### Delete 
sammy=# DELETE FROM playground WHERE type = 'slide';  
#### Alter by adding / dropping a column
sammy=# ALTER TABLE playground ADD last_maint date;   
sammy=# ALTER TABLE playground DROP last_maint;  
#### Update Data in a Table 
sammy=# UPDATE playground SET color = 'red' WHERE type = 'swing';  

### Install PostgreSQL Server on Ubuntu
https://tecadmin.net/install-postgresql-server-on-ubuntu/

#### Import GPG key for Postgres packages
sudo apt-get install wget ca-certificates  
wget --quiet -O -https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -   
#### Add the repo to your system 
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'  

##### Install Postgres on Ubuntu
sudo apt-get update  
sudo apt-get install postgres postgresql-contrib  

#### Connect to Postgres
sudo su - postgres  
psql  

#### Check login info 
postgres-# \conninfo  
postgres-# \q  

### Install pgAdmin4 on Ubuntu
https://tecadmin.net/install-pgadmin4-on-ubuntu/  
sudo apt-get install pgadmin4 pgadmin4-apache2  
```
Use servers IP address or domain name followed by /pgAdmin4 as subdirectory URL.
```

### Install phpPgAdmin on Ubuntu 
https://tecadmin.net/install-phppgadmin-in-ubuntu/  
**Requires PHP and Apache2 to be installed**  
sudo apt-get update  
sudo apt-get install php-pgsql  
sudo apt-get install phppgadmin  
sud vi /var/lib/pgsql/data/postgresql.conf  
sudo vi /var/lib/pgsql/data/pg_hba.conf  
sudo vi /var/www/html/phpPgAdmin/conf/config.inc.php  
conf['extra_login_security'] = false;  
http://localhost/phppgadmin  
sudo nano /etc/apache2/conf-enabled/phppgadmin.conf  
sudo service apache2 reload  

### Remove Postgres
https://www.liquidweb.com/kb/how-to-remove-postgresql/  
#### List the postgres packages 
dpkg -l | grep postgres  
#### Delete the Postgres packages 
apt-get --purge remove command  
sudo apt-get --purge remove _(list of packages like : pgdg-keyring postgresql-10 postgresql-client-10 postgresql-client-common postgresql-common)_  
#### Verifying the Deletion of Postgres
root@newclient:~# dpkg -l | grep postgres  
root@newclient:~#  

#### Delete the database and directories at the same time 
rm -rf /usr/local/pgsql  
#### Remove the postgres user account 
userdel postgres  

#### Simplest way is :
sudo apt-get --purge remove postgresql  
--OR, delete it in parts--  
dpkg -l | grep postgres  
sudo apt-get --purge remove _postgresql postgresql-doc postgresql-common ..._  
sudo deluser postgres  
#### Completely remove postgres in terminal 
sudo apt-get --purge remove postgresql\*  
--OR--  
sudo apt-get --purge remove postgresql*  

#### Confirm 
whereis postgres  
whereis postgresql  

#### Misc.
psql -V  
locate bin/psql  
=# SELECT version();  
=# SHOW server_version;  

#### Get back to Ubuntu root user 
\q to quit out of postgres  
su joseph _(my ubuntu id)_  --> enter joseph pw  


# GraphQL
https://news.ycombinator.com/item?id=14676531  
https://news.ycombinator.com/item?id=14746169  
https://www.freecodecamp.org/news/graphql-for-front-end-developers-1f59808f4435/  
https://medium.com/free-code-camp/graphql-for-front-end-developers-1f59808f4435  
https://thenewstack.io/graphql-data-query-language-resource-guide/  
https://blog.digitalocean.com/learning-graphql-by-doing/  
https://leapgraph.com/graphql-postgresql-postgraphile  
https://graphql.org/learn/queries/#directives  
https://blog.apollographql.com/the-anatomy-of-a-graphql-query-6dffa9e9e747  
https://blog.apollographql.com/batching-client-graphql-queries-a685f5bcd41b  
https://github.com/graphql/graphql-spec/issues/375  

https://stackoverflow.com/questions/51863657/graphql-querying-with-dynamic-filtering-criteria  
https://stackoverflow.com/questions/34321688/can-graphql-return-aggregate-counts  
https://stackoverflow.com/questions/35036221/support-of-aggregate-function-in-graphql  
https://stackoverflow.com/questions/47331391/graphql-return-calculated-type-dependent-on-argument  
https://stackoverflow.com/questions/40945940/graphql-queries-with-tables-join  
