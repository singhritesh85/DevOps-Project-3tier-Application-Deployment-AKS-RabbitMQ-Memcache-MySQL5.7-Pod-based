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
![image](https://github.com/user-attachments/assets/bd27957f-f652-43cb-83cc-3921d7491845)

Create Ingress rule using the rabbitmq-ingress-rule.yaml file as present in this repository and do the entry in Azure dns-zone for Hosts corresponding to IP Address as shown below.
![image](https://github.com/user-attachments/assets/b3728c66-ed7f-4a0d-b852-6200b1b07757)
![image](https://github.com/user-attachments/assets/21d526bb-0287-4592-985d-70cb7dc93236)

![image](https://github.com/user-attachments/assets/1eb56622-4e69-42a3-87d3-539451b7cf66)

The source code is present in Azure Repos. I have taken the Source Code present in GitHub Repository https://github.com/singhritesh85/Three-tier-WebApplication.git and did changes in pom.xml, Dockerfile, application.properties as shown below.

![image](https://github.com/user-attachments/assets/332216d0-24a4-4eaa-b249-f1b007ff88bd)
![image](https://github.com/user-attachments/assets/af8db4fb-858e-4e89-82fb-d709ae466384)
![image](https://github.com/user-attachments/assets/5fbdc0e4-41ce-4667-882e-008b4f1584e0)
![image](https://github.com/user-attachments/assets/04ec02ed-8869-4a21-943b-fe6edcd0e153)

For Azure Artifacts, copy below section and paste to pom.xml as I have shown in above screenshot.

![image](https://github.com/user-attachments/assets/85211167-2c8d-42ac-9abf-1f2afc75a0c3)
![image](https://github.com/user-attachments/assets/54116019-5acf-4113-892d-1f24d77df833)

Provide Contributor Access for Azure Artifacts as shown in screenshot below.

![image](https://github.com/user-attachments/assets/04a18c4a-ba4c-437a-8407-ce6fcdcdf297)
![image](https://github.com/user-attachments/assets/8386d5e4-30fa-4466-a9cb-08226c51968b)

I have created a mysql image and kept it in Docker Hub Repository. While creating mysql pod I imported the .sql extension file db_backup.sql. The Dockerfile to achieve this is as shown below.

![image](https://github.com/user-attachments/assets/dc716dc8-ad26-4f5c-9aba-539541d0f27c)
In Dockerfile as shown above in the attached screen-shot, **docker-entrypoint-initdb.d** directory contains the shell-script and .sql extension file which will get executed when container will be created for the first time. You can consider it as for keeping the bootstraping script.
![image](https://github.com/user-attachments/assets/dc58a6d2-c099-4a2d-aa8f-2e5c283d77f2)

I have created Service service Connection for SonarQube, Azure Artifacsts, Azure Container Registries and DockerHub Repository as shown below.
![image](https://github.com/user-attachments/assets/562f2c17-36b2-46aa-bf8d-a384a08d7b58)

![image](https://github.com/user-attachments/assets/6ad8fe74-c890-4983-bfdc-913048548127)

Now Run the Azure Pipeline. Create the URL using ingress rule for service present in the file ingress-rule.yaml in this repository. Do the entry for this URL with Public IP in Record Set of Azure DNS Zone. Access the newly created URL and provide username admin_vp and password admin_vp.
![image](https://github.com/user-attachments/assets/25779d9e-fb01-4f73-8c39-fe26dd71098c)
![image](https://github.com/user-attachments/assets/5eb1099f-5313-4581-85f3-0e96f4bd2fc3)
![image](https://github.com/user-attachments/assets/d976120b-3e00-4dab-b55a-7845a8f29141)
![image](https://github.com/user-attachments/assets/6116cfd9-e4d1-42de-9727-6a3ebbf20328)

When you click on the User for the first time it will get the values from MySQL Database and store it in Memcache, so that next time when you click on the same user it will provide the values from the Memcache itself.

![image](https://github.com/user-attachments/assets/733d0066-1e87-49b5-8a1a-71a15b0bb6b5)
![image](https://github.com/user-attachments/assets/305af053-497a-4fcd-8f2a-53b78093dfa2)

After running the Azure Pipeline Screenshots for RabbitMQ, SonarQube, Azure Artifacts are as shown in the Screenshot below.

![image](https://github.com/user-attachments/assets/a97ce945-ee97-4b91-bb55-47b0fd0fc44e)
![image](https://github.com/user-attachments/assets/45b8bb3a-ee50-4c8a-b6b2-41271170e3e4)
![image](https://github.com/user-attachments/assets/b67dc904-0600-4382-ae74-29fd908e02dd)
![image](https://github.com/user-attachments/assets/fe902fdb-18dc-4acd-8fb6-388ab414ba1f)
![image](https://github.com/user-attachments/assets/64b626c3-e876-405c-911b-461ac1925520)

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
