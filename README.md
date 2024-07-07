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
![image](https://github.com/kamalmohan217/DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL/assets/128888356/fb065b10-4738-4b0d-91fb-fea5b9e910a2)
