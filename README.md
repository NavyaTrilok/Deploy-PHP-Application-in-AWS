# Deploy-PHP-Application-in-AWS


**Abstract**

A handful of the guided labs and challenge labs that we have already completed in our earlier tasks are effectively combined in this capstone project. 
The primary objectives of the capstone project are to deploy the PHP application (web app) that runs on EC2 and to create an RDS database for the PHP application 
to query and interact with it. Then we need to secure the application to prevent public access to the backend database systems and update the necessary 
parameters in the AWS parameter store. For the web application to scale in response to user demand, we need to build an auto-scaling group and load balancer. 
We must utilize the four parameters listed in the AWS lab instructions to connect to the database. We are also provided with an SQL dump and the PHP files that 
can be used to build the solution.

<img width="472" alt="image" src="https://github.com/NavyaTrilok/Deploy-PHP-Application-in-AWS/assets/14071348/cfc0194e-54ec-43ca-8508-b5e856b70817">

**Summary**

A handful of the already-installed services, including VPC (Example VPC), subnets (two public subnets and two private subnets) security groups (Inventory-App, Bastion-SG, ALBSG, and Example-DBSG), and AMI, need to be inspected first before proceeding with the actual deployment process. Here, I have noted down the subnet ids and IP addresses of the subnets so that we can utilize them in the upcoming steps. 

As a part if capstone project, the project was to deploy a PHP application that facilitates the high user availability for a social research organization. 
Once the lab started I was give when the below subnets and their ip addresses -

Public subnet 1 10.0.0.0/24
Public subnet 2 10.0.1.0/24
Private subnet 1 10.0.2.0/23
Private subnet 2 10.0.4.0/23

The security groups were configured –
Inventory-App
ALBSG
Example-DBSG
I configured Bastion-SG

**Providing High availability to the website through Load balancers –**
Providing anonymous access to web users
I created the application load balancer in public subnet and used ALB security group to enable anonymous access for website users. My Load balancer’s name is NavyaELB. Hence my url to access the application is - NavyaELB-1657184153.us-east-1.elb.amazonaws.com

**Providing automatic scaling that uses a launch template -**
Using Amazon Linux AMI base image, I created the Launch template with public subnet 1 and public subnet 2. I choose the target group as load balancer target group. To this Launch template I have attached the existing load balancer. I have given the health check grace period as 300 seconds. While creating the Launch template I have created the Auto Scaling group for which I have set the size of the Auto Scaling group by changing the desired capacity to 1, minimum desired to 1 and maximum desired to 2 to scale the web servers based on the target tracking scaling group. The Autoscaling group was created and used launch template to initiate 2 web-instances. I managed to keep the CPU utilization at twenty-five percent and scale the instances.
The launch template installed the necessary MySQL packages and application code when initializing. 

**Providing secure hosting of the MySQL database -**
I have created RDS relational database server in MySQL. Before creating RDS, we need to create subnet group. I have created DB subnet Group in private subnets and two availability zones. I have created RDS with MySQL. To ensure security, I placed the MySQL database in a private subnet attached to the database security group. Created inbound rule that allows traffic only from the application security group web servers.
I have created RDS with MySQL with Dev/Test template, set the credentials for the master database, selected the instance configuration db.t3.micro and allocated the storage of 20 GiB for General Purpose SSD(gp2). 

**Updating application parameters in an AWS Systems Manager Parameter Store -**
As the RDS was being created I have created the below parameters in the parameter store –
I have updated system parameters in parameter store. Below are the parameters –

/example/database - exampledb
/example/username  -  admin
/example/password  - My password
/example/endpoint  -  This is the RDS endpoint 
example.cunl7nhfepoo.us-east1.rds.amazonaws.com

Run the website on a t2.micro EC2 instance, and provide Secure Shell (SSH) access to administrators
To connect to the Linux AMI I have added the inbound rules with type SSH and bastion security group.
Once connected to the EC2 instance with its IPV4 address, I have dumped the data to RDS server.

**I was able to access the website with all the data required.**

<img width="472" alt="image" src="https://github.com/NavyaTrilok/Deploy-PHP-Application-in-AWS/assets/14071348/0abd3d30-2eba-49e5-acba-4a7ea010ca5a">

<img width="470" alt="image" src="https://github.com/NavyaTrilok/Deploy-PHP-Application-in-AWS/assets/14071348/99dc14f9-1b9e-4d52-9b57-3c9a8b742964">

<img width="470" alt="image" src="https://github.com/NavyaTrilok/Deploy-PHP-Application-in-AWS/assets/14071348/b79f1a0b-9141-4fdc-998f-8c7a344bac9b">

<img width="470" alt="image" src="https://github.com/NavyaTrilok/Deploy-PHP-Application-in-AWS/assets/14071348/7f6bc120-335c-4f36-b2aa-5fc8f1dcfd5b">




