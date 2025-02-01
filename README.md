**VPROFILE PROJECT SETUP**

**Prerequisites**
1. Oracle VM Virtualbox
2. Vagrant
3. Vagrant plugins
   Execute below command in your computer to install hostmanager plugin
   $ vagrant plugin install vagrant-hostmanager
4. Git bash or equivalent editor

**Technologies**
- Spring MVC
- Spring Security
- Spring Data JPA
- Maven
- JSP
- MySQL

**VM SETUP**
1. Clone source code.
2. Cd into the repository.
3. Switch to the local branch.
4. cd into vagrant/Manual_provisioning

Bring up vm’s
$ vagrant up
NOTE: Bringing up all the vm’s may take a long time based on various factors.
If vm setup stops in the middle run “vagrant up” command again.
INFO: All the vm’s hostname and /etc/hosts file entries will be automatically updated.

**Architecture of Project Services**
![image](https://github.com/user-attachments/assets/0c5057cd-8666-4512-bd1e-abf970bf5082)

**PROVISIONING**
**Services**
1. Nginx => Web Service
2. Tomcat => Application Server
3. RabbitMQ => Broker/Queuing Agent
4. Memcache => DB Caching
5. ElasticSearch => Indexing/Search service
6. MySQL => SQL Database

**Setup should be done in below mentioned order**
MySQL (Database SVC)
Memcache (DB Caching SVC)
RabbitMQ (Broker/Queue SVC)
Tomcat (Application SVC)
Nginx (Web SVC)

Login to the db vm
->	vagrant ssh db01


Login to the db vm

Login to the db vm
•	vagrant ssh db01

Verify Hosts entry, if entries missing update the it with IP and hostnames
•	cat /etc/hosts


Verify Hosts entry, if 

Login to the db vm

vagrant ssh db01

Verify Hosts entry, if entries missing update the it with IP and hostnames


Update OS with latest patches

1. MYSQL Setup 

Login to the db vm 

vagrant ssh db01 


Verify Hosts entry, if entries missing update the it with IP and hostnames 
cat /etc/hosts 

 Update OS with latest patches 
dnf update-y 

 Set Repository 
dnf install epel-release-y 

 Install Maria DB Package 
dnf install git mariadb-server-y 

 Starting & enabling mariadb-server 
systemctl start mariadb 
systemctl enable mariadb

**1. MYSQL Setup**

Login to the db vm

-vagrant ssh db01

Verify Hosts entry, if entries missing update the it with IP and hostnames

-cat /etc/hosts
Update OS with latest patches
-dnf update -y
Set Repository
-dnf install epel-release -y
Install Maria DB Package
# dnf install git mariadb-server -y
Starting & enabling mariadb-server
# systemctl start mariadb
# systemctl enable mariadb
RUN mysql secure installation script.
# mysql_secure_installation
NOTE: Set db root password, I will be using admin123 as password
![image](https://github.com/user-attachments/assets/d7dd153e-3c24-4fec-85b3-8eec38226472)
Set DB name and users.
# mysql -u root -padmin123
mysql> create database accounts;
mysql> grant all privileges on accounts.* TO 'admin'@'localhost' identified by
'admin123';
mysql> grant all privileges on accounts.* TO 'admin'@'%' identified by 'admin123';
mysql> FLUSH PRIVILEGES;
mysql> exit;
Download Source code & Initialize Database.
# cd /tmp/
# git clone -b local https://github.com/hkhcoder/vprofile-project.git
# cd vprofile-project
# mysql -u root -padmin123 accounts < src/main/resources/db_backup.sql
# mysql -u root -padmin123 accounts
mysql> show tables;
mysql> exit;
Restart mariadb-server
# systemctl restart mariadb
Starting the firewall and allowing the mariadb to access from port no. 3306
# systemctl start firewalld
# systemctl enable firewalld
# firewall-cmd --get-active-zones
# firewall-cmd --zone=public --add-port=3306/tcp --permanent
# firewall-cmd --reload
# systemctl restart mariadb
![image](https://github.com/user-attachments/assets/c3970f22-4979-4e60-9d3a-4ab0f0f3e717)

**2.MEMCACHE SETUP**
Login to the Memcache vm
$ vagrant ssh mc01
Verify Hosts entry, if entries missing update the it with IP and hostnames
# cat /etc/hosts
Update OS with latest patches
# dnf update -y
Install, start & enable memcache on port 11211
# sudo dnf install epel-release -y
# sudo dnf install memcached -y
# sudo systemctl start memcached
# sudo systemctl enable memcached
# sudo systemctl status memcached
# sed -i 's/127.0.0.1/0.0.0.0/g' /etc/sysconfig/memcached
# sudo systemctl restart memcached
Starting the firewall and allowing the port 11211 to access memcache
# systemctl start firewalld
# systemctl enable firewalld
# firewall-cmd --add-port=11211/tcp
# firewall-cmd --runtime-to-permanent
# firewall-cmd --add-port=11111/udp
# firewall-cmd --runtime-to-permanent
# sudo memcached -p 11211 -U 11111 -u memcached -d
![image](https://github.com/user-attachments/assets/28bfbe3d-bfb0-4fb3-8380-aca5cfadc890)

**3.RABBITMQ SETUP**
Login to the RabbitMQ vm
$ vagrant ssh rmq01
Verify Hosts entry, if entries missing update the it with IP and hostnames
# cat /etc/hosts
Update OS with latest patches
# dnf update -y
Set EPEL Repository
# dnf install epel-release -y
Install Dependencies
# sudo dnf install wget -y
# dnf -y install centos-release-rabbitmq-38
# dnf --enablerepo=centos-rabbitmq-38 -y install rabbitmq-server
# systemctl enable --now rabbitmq-server
Setup access to user test and make it admin
# sudo sh -c 'echo "[{rabbit, [{loopback_users, []}]}]." > /etc/rabbitmq/rabbitmq.config'
# sudo rabbitmqctl add_user test test
# sudo rabbitmqctl set_user_tags test administrator
# rabbitmqctl set_permissions -p / test ".*" ".*" ".*"
# sudo systemctl restart rabbitmq-server
Starting the firewall and allowing the port 5672 to access rabbitmq
# sudo systemctl start firewalld
# sudo systemctl enable firewalld
# firewall-cmd --add-port=5672/tcp
# firewall-cmd --runtime-to-permanent
# sudo systemctl start rabbitmq-server
# sudo systemctl enable rabbitmq-server
# sudo systemctl status rabbitmq-server
![image](https://github.com/user-attachments/assets/32191d48-a57c-410d-a488-cac316b54543)

**4.TOMCAT SETUP**
Login to the tomcat vm
$ vagrant ssh app01
Verify Hosts entry, if entries missing update the it with IP and hostnames
# cat /etc/hosts
Update OS with latest patches
# dnf update -y
Set Repository
# dnf install epel-release -y
Install Dependencies
# dnf -y install java-17-openjdk java-17-openjdk-devel
# dnf install git wget -y
Change dir to /tmp
# cd /tmp/
Download & Tomcat Package
# wget
https://archive.apache.org/dist/tomcat/tomcat-10/v10.1.26/bin/apache-tomcat-10
.1.26.tar.gz
# tar xzvf apache-tomcat-10.1.26.tar.gz
Add tomcat user
# useradd --home-dir /usr/local/tomcat --shell /sbin/nologin tomcatCopy data to tomcat home dir
# cp -r /tmp/apache-tomcat-10.1.26/* /usr/local/tomcat/
Make tomcat user owner of tomcat home dir
# chown -R tomcat.tomcat /usr/local/tomcat
Setup systemctl command for tomcat
Create tomcat service file
# vi /etc/systemd/system/tomcat.service
Update the file with below content
![image](https://github.com/user-attachments/assets/f61548ff-ba9d-4cb5-85b7-ccf3cb74fa22)
# systemctl daemon-reload
Start & Enable service
# systemctl start tomcat
# systemctl enable tomcat
Enabling the firewall and allowing port 8080 to access the tomcat
# systemctl start firewalld
# systemctl enable firewalld
# firewall-cmd --get-active-zones
# firewall-cmd --zone=public --add-port=8080/tcp --permanent
# firewall-cmd --reload
![image](https://github.com/user-attachments/assets/ec19a395-96aa-4670-8997-317f011bc1e2)

**CODE BUILD & DEPLOY (app01)**
Maven Setup
# cd /tmp/
# wget
https://archive.apache.org/dist/maven/maven-3/3.9.9/binaries/apache-maven-3
.9.9-bin.zip
# unzip apache-maven-3.9.9-bin.zip
# cp -r apache-maven-3.9.9 /usr/local/maven3.9
# export MAVEN_OPTS="-Xmx512m"
Download Source code
# git clone -b local https://github.com/hkhcoder/vprofile-project.git
Update configuration
# cd vprofile-project
# vim src/main/resources/application.properties
# Update file with backend server details
Build code
Run below command inside the repository (vprofile-project)
# /usr/local/maven3.9/bin/mvn install
Deploy artifact
# systemctl stop tomcat
# rm -rf /usr/local/tomcat/webapps/ROOT*
# cp target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
# systemctl start tomcat
# chown tomcat.tomcat /usr/local/tomcat/webapps -R
# systemctl restart tomcat

**5.NGINX SETUP**
Login to the Nginx vm
$ vagrant ssh web01
$ sudo -i
Verify Hosts entry, if entries missing update the it with IP and hostnames
# cat /etc/hosts
Update OS with latest patches
# apt update
# apt upgrade
Install nginx
# apt install nginx -y
Create Nginx conf file
# vi /etc/nginx/sites-available/vproapp
Update with below content
![image](https://github.com/user-attachments/assets/71d85599-3013-4c96-bc2c-c146d61e7654)
Remove default nginx conf
# rm -rf /etc/nginx/sites-enabled/default
Create link to activate website
# ln -s /etc/nginx/sites-available/vproapp /etc/nginx/sites-enabled/vproapp
Restart Nginx
# systemctl restart nginx
![image](https://github.com/user-attachments/assets/70f23286-77ce-44df-81cf-63ab8b41ca5f)

**Final Result:**
We've successfully hosted the V-Profile application Webpage.
![image](https://github.com/user-attachments/assets/7dc064b2-27d9-4977-a615-9efb41dbbcf4)









  



