# DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL5.7-Pod-based

![image](https://github.com/user-attachments/assets/960a4b1b-da57-4060-9cee-423ebb642010)

In the Architecture diagram shown above a basic architecture of three-tier application is shown. In this diagram the first layer or tier is Presentation Layer or web tier. The second layer or tier is Application Layer or Business Tier and third layer or tier is Database Layer or Database tier. For web tier Nginx Service, for Application tier Tomcat and for Database tier MySQL and Memcache(for cache purpose) has been used as shown in the Architecture diagram above.

![image](https://github.com/user-attachments/assets/d8d3f434-8c8c-4b9c-a46d-35c4e7af36b9)
![image](https://github.com/user-attachments/assets/e2a96205-a73b-4134-a179-baad9b930958)

Architecture diagram for three-tier Application and three-tier Application deployment is as shown above.

I have used terraform script as provided in this repository to create the Azure VMs, Application Gateways, AKS and Azure Database for PostgreSQL Flexible Servers.

I have created RabbitMQ cluster with three kubernetes pods. Follow below procedures to achieve the same.

Create Azure DevOps pipeline for deployment of Tomcat Application Pods, RabbitMQ Pods, Memcached Pods and MySQL Pods using the azure-pipelines.yaml file prresent with this repository.
```
Add a user and permission to the user in RabbitMQ. Assign Role for High Availability (HA) using the commands as shown below.

rabbitmqctl add_user test test
rabbitmqctl set_user_tags test administrator
rabbitmqctl set_permissions -p / test ".*" ".*" ".*"
rabbitmqctl set_policy ha-all ".*" '{"ha-mode":"all","ha-sync-mode":"automatic"}' 
```
![image](https://github.com/user-attachments/assets/e4648891-b703-4efd-8bae-f76afd5b3256)

Create Ingress rule using the rabbitmq-ingress-rule.yaml file as present in this repository and do the entry in Azure dns-zone for Hosts corresponding to IP Address as shown below.
![image](https://github.com/user-attachments/assets/7dd2c517-e3a0-4a77-90c3-836cb8b72d82)
![image](https://github.com/user-attachments/assets/82c379f4-36ea-45b4-8e16-024433e87860)

![image](https://github.com/user-attachments/assets/34e243b3-985c-483d-bc8c-f3361d825f05)

The source code is present in Azure Repos. I have taken the Source Code present in GitHub Repository https://github.com/singhritesh85/Three-tier-WebApplication.git and did changes in pom.xml, Dockerfile, application.properties as shown below.

![image](https://github.com/user-attachments/assets/20d8d1ac-b7c5-4ecd-850a-bc0ba766bfad)
![image](https://github.com/user-attachments/assets/c027ad1c-8a88-4f56-838c-659ce79465e8)
![image](https://github.com/user-attachments/assets/9ca4d1bb-0976-4293-841b-db7ba7b75917)
![image](https://github.com/user-attachments/assets/19fde3a2-a1e2-464f-95d7-621c0cc25509)

For Azure Artifacts, copy below section and paste to pom.xml as I have shown in above screenshot.

![image](https://github.com/user-attachments/assets/5b1b7103-e06b-49ca-8522-9da5c8484d08)
![image](https://github.com/user-attachments/assets/04b417a3-c738-4a7b-bab7-df23e16c471c)

Provide Contributor Access for Azure Artifacts as shown in screenshot below.

![image](https://github.com/user-attachments/assets/69d235a4-4d1d-4bcc-87d1-29c87629c5f2)
![image](https://github.com/user-attachments/assets/146adf49-5f7b-4f1f-a565-2a994e50561d)

I have created a mysql image and kept it in Docker Hub Repository. While creating mysql pod I imported the .sql extension file db_backup.sql. The Dockerfile to achieve this is as shown below.

![image](https://github.com/user-attachments/assets/0a7df3a3-2c4b-48c3-b85d-69275bc3635e)
In Dockerfile as shown above in the attached screen-shot, **docker-entrypoint-initdb.d** directory contains the shell-script and .sql extension file which will get executed when container will be created for the first time. You can consider it as for keeping the bootstraping script.
![image](https://github.com/user-attachments/assets/679ee830-2eb1-4c2e-a3a2-44ebd57f4146)

I have created Service service Connection for SonarQube, Azure Artifacsts, Azure Container Registries and DockerHub Repository as shown below.
![image](https://github.com/user-attachments/assets/04bc97ba-5be1-4506-8f0e-33e8ed20360f)

![image](https://github.com/user-attachments/assets/e5e0f56b-8624-43d3-bed7-cc26d8cb287d)

Now Run the Azure Pipeline. Create the URL using ingress rule for service present in the file ingress-rule.yaml in this repository. Do the entry for this URL with Public IP in Record Set of Azure DNS Zone. Access the newly created URL and provide username admin_vp and password admin_vp.
![image](https://github.com/user-attachments/assets/835a265d-5248-4ae2-b492-e8d11af9c464)
![image](https://github.com/user-attachments/assets/b3099186-58a2-49f9-9c2f-eba79b7127aa)
![image](https://github.com/user-attachments/assets/148f12e0-04fb-466e-9834-8e8b78b10bf7)
![image](https://github.com/user-attachments/assets/baf0a179-943c-48f5-a511-200f90602ad1)

When you click on the User for the first time it will get the values from MySQL Database and store it in Memcache, so that next time when you click on the same user it will provide the values from the Memcache itself.

![image](https://github.com/user-attachments/assets/85d59593-17c0-4281-bcf5-bfad4c82a77e)
![image](https://github.com/user-attachments/assets/f430e429-6586-4d28-926c-ba72091ae667)

After running the Azure Pipeline Screenshots for RabbitMQ, SonarQube, Azure Artifacts are as shown in the Screenshot below.

![image](https://github.com/user-attachments/assets/034e1384-004c-433a-a75d-03b88cae14bd)
![image](https://github.com/user-attachments/assets/6ffb11a1-9f5c-475e-8f54-3102dd961b32)
![image](https://github.com/user-attachments/assets/d7844231-1ec9-4a20-9d3c-6d517dc4acf8)
![image](https://github.com/user-attachments/assets/f4af3bef-c12b-476d-8951-313fe306942a)
![image](https://github.com/user-attachments/assets/61bc04b7-6d0c-4c4d-8a8b-492170a3a4e1)

```
Create secret in Kubernetes for DockerHub Repository access

kubectl create secret docker-registry dockerhub-auth --docker-server=docker.io --docker-username=XXXXXXXXX --docker-password=XXXXXXXXX -n mysql
```

```
Create secret in Kubernetes for Azure Container Registries access

kubectl create secret docker-registry devopsmelacr132827a7-auth --docker-server=https://akscontainer24registry.azurecr.io --docker-username=akscontainer24registry --docker-password=cXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXR -n backend
```
<br><br/>
<br><br/>
<br><br/>
<br><br/>
<br><br/>
<br><br/>
```
Source Code:-  https://github.com/singhritesh85/Three-tier-WebApplication.git
```
<br><br/>
<br><br/>
<br><br/>
<br><br/>
<br><br/>
<br><br/>
```
Reference:-  https://github.com/logicopslab/vprofile-project.git
```
