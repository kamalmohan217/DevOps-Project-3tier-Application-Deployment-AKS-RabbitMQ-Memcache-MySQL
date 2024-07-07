# DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/a2f4aa99-1255-4ec1-ad61-9874c8169096)

In the Architecture diagram shown above a basic architecture of three-tier application is shown. In this diagram the first layer or tier is Presentation Layer or web tier. The second layer or tier is Application Layer or Business Tier and third layer or tier is Database Layer or Database tier. For web tier Nginx Service, for Application tier Tomcat and for Database tier MySQL and Memcache(for cache purpose) has been used as shown in the Architecture diagram above.
<br><br/>
The three tier Application is implemented using AKS as shown in the diagram below. It's Architecture diagram is same as explained above but instead of Nginx Sever, Application Gateway Ingress Controller has been used.
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/8d3f99f9-4425-453c-8c5b-533e7a5858fe)

I have used terraform script as provided in this repository to create the Azure VMs, Application Gateways, AKS and Azure Database for MySQL Flexible Servers.

I have created RabbitMQ cluster with three Azure VMs. Follow below procedures to achieve the same.

Using the above terraform script and the bootstrap script used with terraform three Azure VMs for RabbitMQ will be created along with the Application Gateway.
The Bootstrapping script will install RabbitMQ on three Azure VMs. Then list the plugins, enable rabbitmq_management plugin and restart the rabbitmq-server on the three Azure VMs. To achieve this I have used below commands.
```
(a) rabbitmq-plugins list
(b) rabbitmq-plugins enable rabbitmq_management
(c) systemctl restart rabbitmq-server
Finally you can ensure that rabbitmq_management plugin is enabled or not using the command rabbitmq-plugins list
```
**First Node, Consider it as Node-1**
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/815b7733-bcd2-4911-8e7d-318fff3fd37d)
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/e521f861-c010-4158-988d-f4e1043a94b3)
```
Run below command on Node-1 to set the policy for High Availability (HA) in RabbitMQ Cluster.
rabbitmqctl set_policy ha-all ".*" '{"ha-mode":"all","ha-sync-mode":"automatic"}'
```
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/51104b6e-90d5-4387-bc4c-83380a6f7ba9)
**Second Node, Consider it as Node-2**
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/70c4da4d-acc0-466c-97cc-68033fc3bb75)
**Third Node, Consider it as Node-3**
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/010e351d-39de-4564-a0e1-0eb4d56668f7)

To create RabbitMQ cluster of three nodes follow the below procedures.
```
On Node-1 (rabbitmq-vm-1) open the file /var/lib/rabbitmq/.erlang.cookie using cat command and copy the hash value of cookie and paste it on Node-2 and Node-3 in the file /var/lib/rabbitmq/.erlang.cookie.
Then restart rabbitmq-server service, then stop rabbitmq application using the command rabbitmqctl stop_app on Node-2 (rabbitmq-vm-2) and Node-3 (rabbitmq-vm-3). Finally run the command rabbitmqctl join_cluster rabbit@<IP_Address_Node1> and start the rabbitmq application using the command rabbitmqctl start_app on Node-2 and Node-3.
```
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/6f48f306-2cf1-4756-a28f-e7bfc0293259)
**Node-1**
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/0f00f5d2-5c57-4d62-a12e-9885630bfeba)
**Node-2**
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/e1646940-db19-43d8-a2d1-f5bcd6bab015)
**Node-3**
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/61cdbf7c-71da-4999-ac6a-51a6d158577d)
<br><br/>
Finally copy the Public IP of the Application Gateway of RabbitMQ and create the Record Set in Azure DNS Zone. Access the URL and you will see the default console for RabbitMQ, you can use the initial username and password as guest and login into the RabbitMQ console.
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/4c605c44-2214-4c32-a7bb-515604b7cde1)
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/45f2d439-b994-4795-beb3-14bfed6200b6)

The source code is present in Azure Repos. I have taken the Source Code present in GitHub Repository https://github.com/singhritesh85/Three-tier-WebApplication.git and did changes in pom.xml, Dockerfile, application.properties as shown below.
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/7100eeb4-19b8-4ea2-8d8e-a1b0ab3642fc)
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/e42f9604-3bc5-4351-9923-9263a10b0cd0)
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/dd835d97-f514-40c0-b73b-040ba07c4f29)
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/5a11c2b3-be21-4493-9a0c-eb049d0a5868)
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/373ca2f4-8c8c-4248-9d99-c8f77796aef4)

For Azure Artifacts, copy below section and paste to pom.xml as I have shown in above screenshot.
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/4c267e96-9a13-4273-ba43-cb2ff6b016af)
Provide Contibutor Access for Azure Artifacts as shown in screenshor below.
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/5f4a74f3-a816-4ea5-a28f-831a41c14a65)
<br><br/>
Create a database named as account in mysql and import the file db_backup.sql. 
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/7867a9fc-c126-48be-ae19-21f8312876c1)

I have created Service service Connection for SonarQube, Azure Artifacsts and Docker Repository as shown below.
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/c61321d3-a347-4420-93ea-8f6334d0ac82)


<br><br/>
After running the Jenkins Job the Screenshot for RabbitMQ, SonarQube, Nexus Artifactory and ArgoCD is as shown in the Screenshot below.
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/72b6b89b-b9cc-407a-afa1-7887412963bc)
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/c384ae7f-a53a-4022-a5cb-e48d207c8b56)
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/e2335b90-2d3c-40f8-8d71-a923add72b5e)
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/be119104-1e90-4486-a202-8019b891fe25)

