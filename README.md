**Title**: Setup a Multi Tier Web Application stack by using Vagrant.

**Prerequisites**
1. Oracle VM Virtualbox
2. Vagrant
3. Vagrant plugins
   Execute the command in your computer to install hostmanager plugin
   ->vagrant plugin install vagrant-hostmanager
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
-> vagrant up

NOTE: Bringing up all the vm’s may take a long time based on various factors.
If vm setup stops in the middle run “vagrant up” command again.

INFO: All the vm’s hostname and /etc/hosts file entries will be automatically updated.

**Architecture of the Services**
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
1. MySQL (Database SVC)
2. Memcache (DB Caching SVC)
3. RabbitMQ (Broker/Queue SVC)
4. Tomcat (Application SVC)
5. Nginx (Web SVC)

**Final Result:**
After activating all the above services by using Vagrant, We've successfully hosted the V-Profile application Webpage.
![image](https://github.com/user-attachments/assets/90228965-1025-45cd-967d-49638c0e2597)
![image](https://github.com/user-attachments/assets/ec21c7d5-d71a-4ac0-b817-ddfb262bc7f1)
![image](https://github.com/user-attachments/assets/290a89a4-4900-4b8c-8124-e48a3c28e0a1)
![image](https://github.com/user-attachments/assets/57739a79-f17a-49b8-8e50-b76028d9bf51)
![image](https://github.com/user-attachments/assets/81fbe039-f614-4f74-8157-fe429ecc5f33)















  



