### 119. Introduction
- In this project, we will re architecture or refactor our services. This strategy is called re architecture or refactoring. This approach is used to boost agility or to improve business continuity. So we can add new features, scale effectively and easily and have very good performance for our application workload.
- So we can add new features, scale effectively and easily and have very good performance for our application workload.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/1828edab-5c90-478e-810e-fff4683b0e9c)

- So let's take a scenario, let's say you're working on a project where the services are running on physical machine, virtual machines or even cloud machines, like could be even ec2 instances. And you're dealing with various services for your application workload. You could be having databases, you could be having application, webservers, network services like DNS, DHCP and many more services.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/88f598c8-cf71-4579-a619-fc6b6dad8e8b)

- To manage all this, you need you need multiple teams, you need cloud computing team if you're using cloud computing platforms. You need virtualization team if you're doing virtualization on your data center, datacenter operations team, monitoring team, sys admin team and few other teams will get involved in managing all this application workload. So there's really too much operational overhead.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/0a622777-0afd-49d3-8f64-088d1684ff09)


- Your teams are struggling for the uptime and regular scaling requirement. Upfront capital expenditure and regular operational expenditure, if you're using your own local data center and the processes will be manual and will be very difficult to automate.
- If you have virtualization. All these processes will be time consuming and very expensive.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/24a9edb8-0677-460f-984a-d62351bed0f5)

- So we can really use a cloud platform, but instead of using infrastructure as a service, will be using mostly PASS and SAAS services, `platform as a service` and `software as a service`. So if you're talking about AWS, we will not be going with regular instances, but will be using some cloud managed services from AWS and cloud means we can code our infrastructure so we can have infrastructure as a code.
- And SAAS services are very easy to manage, flexible, elastic in nature. Scaling will be mostly taken care of by the cloud vendor. And of course, it's going to be pay as you go model.

- With lots and lots of automation that we can do, so refactoring our application really gives us an easy infrastructure to manage, very good performance, very convenient to scale, and you will not need huge teams to manage all this. So let's see AWS services that we are going to use in this project.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/50295c61-f2a7-496c-88e8-6511e1499302)

- So instead of using regular ec2 instance to install our services, we will be using Beanstalk service. And the service will in turn create an ec2 instance and host our application on it. We don't need to manage this instance manually.
- Beanstalk service will take care of it. Beanstalk service will also have a `load balancer`. It will also have `auto scaling`.
- An S3 bucket for storing the artifacts. Or we can use even our own extra bucket. Now, that's all about `Frontend`

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/e8a93199-2294-4116-9ce3-cfc52c01770f)

- In Backend, for database we will use `RDS instances`. It's really like a platform as a service, so you get a database platform to choose from, you fill in the requirements and the database is up and running in no time. Scaling will be very easy. Regular backups will be taken automatically. And so many more amazing things with it.
- We're going to use `elasticache service` instead of memcache, `ActiveMQ` in place of RabbittMQ. `Route 53` for DNS. And `cloudfront` for content delivery network. So if you have a global audience, then using cloud front for content delivery network will be very easy and convenient.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/246eb495-2fb8-404f-991f-187e9e770ca3)

- Let's keep the objective in mind. We need a flexible infrastructure, very flexible, actually pay as we go model, infrastructure as a code, and we need PASS and SAAS services for ease of managing our infrastructure. So to have a low operational overhead.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/513a1c6d-b5ae-423b-b34b-9c1f075a27d8)

- So let's do a quick comparison between the services that we are going to use, Beanstalk for Tomcat instance replacement, Beanstalk again will have load balancer and auto scaling. EFS or S3 we can use instead of using NFS. RDS, instead of having MySQL on VM/Ec2. Elasticache instead of memcache and active MQ instead of Rabbitt MQ on VM/Ec2instance. Route 53 for DNS. Cloud Front to serve our global audience content delivery network.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/59a0b89e-fb87-4e0b-804a-ef80540e948b)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/5656bfe4-08c0-4704-88e8-d10f0a89444d)

- Front-end
  - User will access our URL, which will be resolved to an end point from Amazon Route 53.
  - The end point will be off Amazon cloud front content delivery network, which will cache so many things to server the global audience.
  - From there, the request will be redirected to application load balancer, which is part of your elastic Beanstalk, Application load balancer, will forward the request to EC2instance, which is in an auto scaling group. Here our Tomcat application service, will be running and all this will be part of Elastic Beanstalk.
  - There will be also `Amazon cloud watch` alarms that will be monitoring auto scaling group and will scale out and scaling based on the requirement.
  - There will be a bucket where artifacts will be stored and we can deploy our latest artifact by just clicking a button.

- Back-end
  - For Backend instead of rabbit MQ, we're using `Amazon MQ`.
  - Instead of using memcache on ec2 instance, we're using `elasticache`.
  - And instead of using database running on ec2 instance, we are going to use Amazon RDS
 
- So the user will access an end point. That end point will be off Amazon cloud front that will send the request to application load balancer in the Beanstalk that will forward the request to instances in the autoscaling group. All this will be monitored by Amazon cloud, which alarms The artifacts will be stored in the S3 bucket. For backend It's going to access AmazonMQ, Elastic Cache and Amazon RDS Service.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/b64e607f-334c-4a9b-9b14-662888e07669)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/a832b4a2-bfc5-4de1-b0ac-b31e3a8f7cca)


### 120. Security Group And Keypairs
- First thing we'll do is we'll create Key pair, Log-in Key pair for our beanstalk ec2 instance, it's not mandatory to log into Beanstalk ec2 instance, but if you're troubleshooting and we would really like to know what's happening inside the ec2 instance, then we can use the key to login.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/d1db0af1-4ce9-4fef-8295-8b9322c9f23a)

- OK, next, we will create a security group for our backend services.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/ea65debd-c9e7-4d75-a460-3e3669c94b28)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/77f38f63-8243-435d-9787-2d2052f9896d)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/504a337c-0d80-43e7-9794-40fe73ea25cf)

- Click create and go back to edit inbound rule

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/13318163-7b21-4d19-8727-6aa701643d5c)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/d4900872-8211-495f-a096-074eb168d3ec)

### 121. RDS
- In this lecture we will create RDS, relational database service. That will be the database service for our project. So search for RDS.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/3e276b64-3848-4e13-b221-c1c83863a659)

- Before we create RDS instance, we will first create the subnet group and the parameters group. Let's click on the subnet group. Now this is not mandatory, but when you have your own customized VPC you can create group of subnets in which you want to create your RDS instance. As of now, we will be just clicking on create subnet group.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/468e15b8-2d92-4eb0-a80c-c82d62f75529)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/2e11028d-c2e9-417d-a30f-23d0ccecee5a)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/872b0d90-ec23-4c79-b3be-71462efc9277)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/9115a33a-aa95-43ab-8cae-88845614da57)

- Now let's go to parameter groups. Now this is for DB geeks who knows the database parameters and they want to change it. Now since RDS you cannot do an SSH login and change the configuration, RDS gives you parameter groups for the database service that you want to create.
- Let's click on create parameter group and here in the dropdown you have to select MySQL

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/a06c3d0f-f8d4-430d-9c1c-93bf520b834f)

- Now let's go to databases and click on create database.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/cb4e71ba-e265-4475-8dca-daee0a89cff6)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/41a06772-90cc-48fc-a190-659dd50005c1)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/014d84c5-37c7-42c4-9f42-575a06f6996f)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/e04dd36d-879f-4f0a-a3ec-349919304ef9)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/6fa1fe51-f2cc-4b33-8b9c-8d4f9db679be)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/db4cc253-0000-4cde-8093-6decc53bce8c)
  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/6bd724d3-ba9b-4ed1-bff6-0ee0e371117a)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/596c0149-5850-45de-9fe6-407f0984f5c2)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/f2e09fd2-d1cf-4a9d-89a6-aebafe6073c1)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/e04c5f78-ec4b-4bf1-8b0a-5bbc725e75a0)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/7ce2b9ab-7792-4514-9685-07eb83dc6a7c)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/450b34fb-b005-4a64-8c2a-66e20e28ffeb)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/fd88b830-3056-4556-b22e-0cb62950ef27)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/638293b4-dde3-490f-9f29-05ccc7a1b479)

- Give a database name `accounts`

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/d286e4bf-d890-4376-8a2d-b10fc766ea8d)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/3fb54863-9dab-4f13-b5f1-e84b05e16b3d)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/ae22be08-a1ac-4322-8425-b56d0552e3c2)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/e4ad7419-2c4b-49a6-a34f-cbc35c0c0517)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/c29452a0-2567-47f0-9c80-0fec5e27744b)

- Click on Close

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/07f668ad-417e-4efb-8375-728f4f93d03f)

- Save username, password and link URL

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/9cdc7eed-7b31-4698-9b2a-d445032d5a04)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/5950aaa5-3824-4349-972d-b8b44d983036)

### 122. Elastic Cache
- Let's create ElastiCache cluster. Not exactly cluster. We'll be creating the single instance to save the cost, But, yes, you can create the cluster also for it. So similar thing, this is going to be much simpler. We are going to create the Parameters group and Subnet groups.
- So let's go to Subnet groups first.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/db525928-603c-4c04-901f-34e9ff5a1566)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/76e83e34-94f6-4974-9294-e2ffdec99638)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/c56f21de-2805-4b57-96fb-1cc45e917460)

- Go to Parameters group and click on Parameter group. Create parameter group.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/e3283a45-1e0e-4412-972e-97453baae1fe)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/9b581d4d-9d10-43b1-ac58-391825fed0fd)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/2ebf93de-4bdb-4cb9-ba73-0ff557309cb2)

- We can go to Dashboard also, Dashboard. And click here on the dropdown create cluster and select Memcached cluster

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/b9d100fc-5282-43cc-bc09-f95842a9e8f4)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/d57f5daa-f2ab-4cc9-b06f-0a99f4cfd683)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/c8463f50-d19e-4e5d-ad7b-7022e5d98168)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/500f2ddb-f4f1-4c95-9213-be2e26760969)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/e53f5bdb-cdec-4ed0-8c00-e77e465ba70c)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/96a6305b-5584-4c9f-8b00-dfe54d52df28)

- Click on Manage and select vprofile-backend security group.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/649508ff-a300-43b2-b0fa-78cb2b17c9b9)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/2ce2abc0-2bae-4714-82c2-59f4f10c0cde)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/5a9864ec-62d3-4bd1-83ea-8db9e213637f)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/97e8b7b1-f0f4-4f57-a8dc-c7b5b856b9be)

- Check all the details, especially the Node type, t2micro, and also your Security group, and click on Create. Now, this also takes a very long time to create the cluster. So by the time we will create a RabbitMQ.

### 123. Amazon MQ
- So we will create the RabbitMQ service and its name is Amazon MQ in AWS. So find it, open it.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/6c36a311-3e4a-4649-a8b8-e44f0fb89ed0)

- Amazon MQ, fully managed open source message broker service. Let's directly go on get started.

- Our requirement is RabbitMQ. Step 1

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/206f45bd-80b0-48ee-a70b-fdcd6c751a35)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/edeb52fc-a5e7-4caa-bc6e-915d298eaf05)

- Step 2

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/ee50c2d6-f030-4ea4-8ac2-8a2d8781e82b)

- Make sure to take note password and username

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/e7a45916-5d23-416f-acaa-7286bc7b6007)

- So let's keep the broker engine default. You have other versions as well. If you want to stream the logs to CloudWatch Logs, you can enable that. Network and security, we'll keep private access because our Beanstalk instance is going to access it in the same VPC. It's going to be private connection, so we don't need to enable public access.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/590a7b2c-e41c-4a62-ab8c-c9132f344ce6)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/a35261ad-0144-438f-8b58-89cb14674b96)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/21f65e08-2451-462c-8325-49e46511c340)

- Click on Create a broker

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/f4be2b1b-87bf-4069-9dc6-7a28c490ad0b)

- Now, this is going to take some time to create. By the time, we can go back to the RDS database and see the status is it's available.
 Let's click on that and let's get the endpoint of the RDS instance. Let's copy this and save it in the sticky note

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/b309a358-ddb6-487c-b1a3-8f33b822a384)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/94e0a529-632e-4e51-b281-44a8b71c82ea)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/8c462aa7-5d95-4ea6-95bb-d468aec35ce7)


- Now in the next lecture, we are going to initialize our RDS database instance. That means, we are going to deploy our SQL file into that. So we have to launch an instance in the same VPC and initialize our database from there.

### 124. DB Initialization
- We have some SQL queries to run on our RDS instance. That is the requirement for our vprofile project. The SQL queries are in our [source code](https://github.com/devopshydclub/vprofile-project/blob/aws-Refactor/src/main/resources/db_backup.sql).
- We have the same code so this SQL query is here, SRC, main, resources, Dbbackup.SQL, this file. Now the only way to access our RDS instance which is in private subnets is through the same VPC in which our RDS instance is.
- So we are going to launch an EC2 instance in the same VPC so we can connect our RDS instance and run this SQL query.
- So let's go to EC2 service. Make sure you're in the same region. In this region, we have one default VPC and we'll launch our instance in the same VPC.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/27284a56-be0b-4bc5-b046-5a2b8ef62adf)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/74c6429f-9ad3-4943-928d-874fdf79ca1c)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/f730da32-c972-43d2-8294-191b179cbcfd)

- Create security group. Wait, let's click on Edit.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/41118af4-8fe8-4aa4-a8bf-c6c3beb6a5d8)

- And we will give 22 from my IP so we can SSH.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/91bfc74d-03a8-444e-80d8-2b12621373aa)

- Click on Launch instance

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/60749e3e-d8cc-4e93-8bf9-ef0eca072c1d)

- Now this instance is going to take a moment to launch. By the time we have to enable the backend security group to allow connection from this instance security group. Go to security groups and find here MySQL client security group. Copy this security group ID 

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/98ac115a-7a1b-41a3-baee-a6b9645deaca)

- And then go to vprofile backend security group. Click on Inbound Rules, not outbound. Edit inbound rule

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/041b8578-a01d-4a0b-91b6-442a03ea386a)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/8f2da488-093f-45bc-8eb2-bf4f54b882c1)

- Let's go to our instance. Let's take its public IP. Let's open GIT Bash or terminal. Let's do SSH -I downloads and the key path, vpro-bean-key. And type `sudo apt update && sudo apt install mysql-client -y`. So if you have used Amazon Linux or CentOS, then you have to install MariaDB. The package name is MariaDB. For Ubuntu, it's MySQL-client.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/d65f1e58-8549-42d6-97fb-cc83edc86dd1)

- Okay, let's test the connection. We are going to say MySQL. Let's get the RDS endpoint.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/3528793f-eca0-4247-ac82-3fcbca5f783f)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/8a1ea0e6-8a90-4979-bacf-a0a0c65ff110)

- So let's exit from here and let's clone our source code. Get the URL of your source code and do some steps

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/427b95e2-6367-4603-be77-6767f373a32e)

- Now this command should execute all these SQL queries on the database accounts which is on RDS. Now let's login again to the RDS instance.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/27b565de-979d-42e1-980a-a4057cda9d38)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/468af034-ec40-4902-9ad3-7bdf1dc867fd)

- Then your database is ready. RDS instance is ready to connect. And after this work is done, you can also delete this mysql-client instance.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/f88afb31-6521-4fa3-af7e-c478a2332150)

- Okay, let's take a look at broker whether it's ready or not. It says running. Good. Okay let's get the endpoint, endpoint AMQP.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/f56bb253-f78b-4174-86aa-8528f6dc59bb)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/4c486a69-16b3-485a-990d-26fe25701f3e)

- Copy this and let's keep it in our sticky note. Let's make sure you remove this port number from the end.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/39fb15e6-f4e0-4ecb-b29e-1b0ec6484e5e)


- Also, let's take the endpoint of ElastiCache.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/2e8d0f93-506e-461a-8b75-7a3b38660be0)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/17f8c334-7d51-435b-b246-fa789c8ef034)


- Okay so all our backend is ready to use. Now we will go to Beanstack service and set up our Beanstack environment and we are going to deploy our artifact and from there, it is going to access these backend services.

### 125. Beanstalk
- Elastic Beanstalk is a platform service that gives you a platform, Java, .NET, PHP, Node.js, Python, Tomcat. So you get a ready-made platform, and you can just upload your artifact there and start using it.
- Behind the scenes, it does so many things, and it'll be easy for you to understand because it does similar thing. It creates EC2 instances, it creates load balancers. It stores the artifacts into the S3 buckets, manages the security group, key pair, and all the other things that we generally require to host a web application on EC2 instance, all those things it manages together as a suite. So you get a dashboard for all the settings. You make the settings.
- Behind the scene, it'll take care of load balance or EC2 instance, artifacts, everything. And we'll see how we are going to do it. We're going to learn by doing this by using this Beanstalk.
- But before we create this application, there is recent, some issues happening with Beanstalk and the issue is related to IAM rolling. So Beanstalk assumes some roles. Let's come to IAM. You can go to IAM, click on roles, and roles are set of specific permission, the policies, AWS policies, and we can attach a role to an EC2 instance. We have seen that previously. So Beanstalk will have some roles, it will create its own role, and it will have permission to access backend services.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/98e11041-712b-4437-9666-fd35e49bddff)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/fd4b3f99-6274-46b7-bcab-0b82b33d9379)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/489efaf7-2bb8-4c50-a83f-a29c1f1a860c)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/6a5d521f-5cd3-4b8d-9aa5-d1cfb6601409)
  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/b8823eb3-35cc-4610-9b28-be88eb815017)
  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/15242e56-2ef8-4418-b4bc-6b53045d2cf5)

- So these four policies you need to select. Take a look once again, Web Tier EC2 Role, Administrator Access Beanstalk, and Beanstalk Role SNS.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/58e74753-c958-41ab-a8fb-a8f22b9ef64f)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/34f978c2-6748-4cb5-87c0-3cec11c0c3e6)

- Okay, let's come back to Beanstalk, and let's click on Create Application.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/60d7ae32-683e-401d-9dca-6e25fc2eb274)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/929c428d-e569-4bca-94cf-e8cf65139569)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/06bd265a-f161-459a-bb46-d27c8be79946)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/6d8e6dcd-ecb0-49d5-b587-0a77df55616f)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/eab2b9b2-6f04-4df6-9b3c-ef640e99481f)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/4b4f519b-50a3-4384-a71f-df0a891cabc8)

- Okay, now this is what I was talking about, the roles. So here you need to have two roles. One is a service role and this one is the instance profile. So in the instance profile,  you need to select the role that you created from IAM.
- Here, we are going to say create and use new service role, and it will take this name by default. If you already have a role with this name, you will get an error, so make sure if you have already the role with this name, then now check it, if you have it, delete it. So let it create the role by itself, the service role.
- In the EC2 key pair, select the key that we created previously. This key will be used by us to log into the EC2 instance created by Beanstalk. So Beanstalk will create EC2 instance, and to access it, we need a key, so this will be that key.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/d6cf1b31-2ecd-4426-97a6-f86044a2b450)

- Next. VPC will select the default VPC, but as you see, there is an option so you can create your own VPC and select here as well, but let's keep default for now.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/57852e90-7c18-42f1-9cf0-f3fb64512d59)

- Instance settings. So public IP address, we want our instance to have public IP address, so I'm gonna put a check mark here, activated, and subnet zones. So I'll select all of this.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/4445c2b6-4ebd-4a0b-b283-4d22755ed7f8)

- You have an option to have a RDS database with Beanstalk connected, but those are used only for test and development purpose, not in production. And the reason for that is the lifecycle of the RDS will be tied with the lifecycle of Beanstalk. If you delete Beanstalk, your RDS will also get deleted. That is good for just testing and development purpose, but for production, you should have a separate database, managed separate, its lifecycle should be separate from the application lifecycle. So we are not going to create ideas through Beanstalk. We have created it separately. So let's skip these things.
- Come down, tags, let's give some tag. I'll say name, I'll do it properly.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/db06f68d-4f8e-46d4-902b-ffd28a2ce2b9)

- Route volume, so instance, we'll use some volume, and by default, it's container default. We'll keep that, but if you see there are other options, general purpose, provisioned IOPS, Magnetic. We talked about this in the EBS, so keep it container default.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/b8e1b094-1ba7-4fd2-8a1e-dd26fa57e7e2)

- Come down. Security group, skip this one. We will let it create its own security group. You can put instance in a separate security group also, but let it create its own security group.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/95cfcfa6-54b1-4e92-8313-314bc38cbd7a)

- Capacity, auto scaling capacity, so we'll go from single instance to load balanced, and we'll say, minimum, we want two instances, and maximum, you can have as many as you want, because anyways, we are not going to put any load on this instance, so it is not going to scale out.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/6a6b6208-60eb-4148-a278-f27953132347)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/2060f98e-a616-43cc-a112-73ad6b216b8f)

- Scaling triggers. We have seen the auto scaling group. We used scaling trigger as CP utilization, right? If the CPU spikes more than a certain limit, we launch instances, or auto scaling group launch more instances, or it deletes the instances if CPU comes down. But if you see, or from my own experience, web application, most of the time, we use network out as the metric.
- Network out is traffic that is being served to the users, so more users, more web pages it will serve, and the traffic out going out will swell. So more users, more network out. That means you need more instances. Less user, less network out, less instances.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/16c82adf-eb21-4114-9aa8-69c58a2ec4ee)

- Now is the load balancer network setting, so you want load balancer in which zones, so we'll keep the same zone as the instance

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/d345ac3e-795a-4f74-b45d-204385f95172)

- The load balancer will be application load balancer. Network load balancer is expensive, it's high performing, let's keep it application load balancer.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/7d443acf-fa8b-4333-87b8-15abe608fc45)

- Listener will be port 80, but we'll be changing this later. But let's keep this default for now.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/17f3d2df-fd68-44c6-810a-8a0c1d9e3f37)

- Health also will be changing, health check. Once we deploy our application, we profile it listens, not on slash, but on /login. We'll change that later

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/0d9e9091-ebb0-4cd9-b536-16ed70cc7dc0)

- Come down, log files. If you want, you can store the logs to the S3 bucket, but let's not do that now. And I get this error still exist over here. Let me close this one.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/26b89bbc-b20f-4c04-99b3-918738ae0a8e)

- In the next page. Enhanced, so it'll monitor the health of our application on many metrics, or you can have basic also. We'll keep it enhanced. This is very useful when you are deploying application in an auto scaling group where you have multiple instances. So one at a time, different, based on different policies. Going to check the health of the instance and apply or roll back the deployment based on that, so we need to keep this enhanced.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/6b3a8954-b409-4302-af2d-b922ba3ad3c7)

- Managed platform updates, like in RDS, we have seen you have regular updates. So when do you want to apply those updates? So let's keep it default.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/2fd3f808-1438-43d7-ac42-9c9340598905)

- Okay, this is the most interesting feature of Beanstalk, rolling updates and deployments. Deployments, as the name says, when we deploy our application, when we upload our artifact to Beanstalk, how do you want to deploy it? As in, what policy you want? Do you want to do it all at once?
- So let's say you have 10 instances in the Beanstalk, all at once means all 10 instances will be brought down, upgraded, and then brought up and back to the load balancer. So during the deployment, your application will go down, user will not be able to access it. That's what all at once means, but if you want, you can go with rolling also. So again, you have 10 instances, and use it batch size is 50. That means five instance upgraded at a time. There's not a good number.
- If you really have 10 instances, 10%, that means one instance at a time is a very good number. You don't want the capacity of your application to go down half all of a sudden. There'll be huge lag. So maximum, in the meantime, I have given 30% maximum, but I was always less than 30% and the 10% is the best number, or one instance at a time, right?
- So we have two instance, we selected two instance, so we are going to give 50%, right? So that is, again, one instance at a time. You can give a fixed number of instances also, like one instance at a time. I like this number, one instance at a time always.
- So we say, let's see here, percentage size 50%, so again, you have 10 or two instances, so half of the capacity, it's going to add new instances. It's not going to delete instances first. It's going to add new instances. If the new instances are healthy, then only it will delete the old instances. So at the time of deployment or upgrade, you'll have extra instances, not less instances. When everything becomes okay, then you'll have same number of instances, but the capacity will not go down. It will be always additional at the time of upgrade.
- This is the safest and the most expensive one. So immutable is like this, you have 10 instances, and you want to do a deployment, it's going to launch 10 new instances. That's why I said it's expensive, but it's the safest one, because anything goes wrong in the rolling, if anything goes wrong, it is upgrading the existing instances. Things can go wrong, and you might not be able to recover the old instances. So immutable is safest.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/774bdfb9-c9be-4dfb-90d6-4f1e4e78d020)

- And come down. Keep everything default and go Next.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/fc70b869-4f85-40b0-b624-4c2512b893e9)

- Final

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/8d5645b7-0449-47f7-a98c-bbd5b203273e)

- And this is the URL. If you click on this, going to open it in the new tab. This is the default application that it has deployed.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/0bbd89d7-f191-494d-bf01-02f37987d479)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/b9353787-9c71-4bd3-b62a-1acdd25ed26c)

### 126. Update on Security Group & ELB
- In this lecture we will be doing three things. Number one, we'll enable ACL on S3 bucket. Number two, update the health check in the target group. Number three, update security group.
- So first we will go to S3. You should see here S3 bucket that starts with the name "Elastic Beanstalk" or something like that, and check the region code that we are using. So my beanstalk is in North Virginia, which is US East one. So this is my bucket. Click on this bucket.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/fd3302b1-eba3-4596-9986-358a7a677ebd)

- In this, go to permissions.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/e2e91240-800b-41ea-be45-f321832e4523)

- Object ownership. Click on edit.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/995cb81b-1897-4e6e-830a-f36cff285443)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/30d4c4e7-5986-42ac-a608-d81df8e96943)

- Now ACL, long form access control list, basically gives you a way to add permission to other AWS account or its own AWS account. So we, the root user of this AWS account, can have access to the S3 bucket. We can upload, download files from it. If it is disabled and which is there disabled by default. Recently these changes happened in AWS, that by default the ACLs on the S3 bucket will be disabled. So how do we access S3 bucket? It's only through policies.

- Okay, this is done. Let's come back to the beanstalk. Go to your enironment. Click here on configuration. Scroll down. Here. Instance traffic and scaling. Click on edit.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/00763147-eadd-489f-8c5f-6c919497cd0e)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/f08b16d0-673b-4790-9788-444063578db4)

- For now, come down. And all the way down... Here. Processes. Click on this radio button. Action, edit, health check, path: /login. Vprofile application is accessible on the URL slash login. And if the target group performs health check on this slash login, it'll find it healthy. If it's not, then it'll find the application as unhealthy, even though it is healthy.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/178a5213-b001-479b-ae0d-049423011c66)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/f47951d4-7888-41f3-8ab8-1b448bde47d3)

- After giving this path slash login come down and there is a dropdown here. Sessions. Expand that and say session stickiness enabled. Now this is especially for our Vprofile project, but let me explain to you. What does this really mean? So we have multiple instances in our application. Now, if a user request comes, we log in, the user is interacting, it is going to land in one of the EC2 instances where our application is running.
- If you want to stick that user to that instance then you enable stickiness, because if the user refreshes the page there is a chance it may log in or land into another instance, which is usually not a problem, but for Vprofile application, we have some bugs here. Not exactly bug, but it's designed like that. So a cookie will be stored in your browser, the user's browser, and whenever the user accesses the application, it'll land in the same instance to which it was previously connected. If the cookies, caches are cleared then it may land in any other instance. That is usually not a problem, but for a Vprofile application we need to enable the stickiness and say save.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/9797b2b2-c463-433c-9a30-e5639b56349a)


  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/4c6a747d-c4ab-458d-994d-7ead051e8fe2)

- We will add a listener 443. Connection: HTTPS. Certificate. So we already have a certificate so I'll choose it from the ACM. It's gonna list that and just say save or add. It's not yet created. We have to save all these rules. Come down and click on apply. 

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/650446b7-30c0-42e0-8554-4022aeb6a4bd)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/4c5709ee-25f1-4aee-8a31-d3f626f2726c)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/71a2d764-a6ce-449b-b4ff-ae7679407619)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/4e163407-72d7-4996-8c9c-df47c7ce75c2)

- Let's type here "EC2". Let's open it in a new tab. Okay, so instances running: two, and you should see here the two beanstalk instances, okay? I click on that and click on security. You should see the security group also of this.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/177d003e-becc-4057-90a0-08877d5405d0)

- So copy the security group ID

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/b4d35498-a06e-4058-88da-3c3321171d32)

- Go back to the security groups and now find your Vprofile backend security group here.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/30a22d76-a926-41fb-9176-9197955c776e)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/4a6182de-0913-4164-86d1-6c265148ea21)

- Add rule. Say here, "All traffic". And here, paste the security group ID of the instance.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/6b3cbd03-7a7d-4bde-9ec0-aff07e056782)

- Allow all traffic from instance security group. Now, why I did this, let me explain. So our backend services, RDS, ElastiCache, Amazon MQ, these backend services are in the backend security group. Instance of the beanstalk will access backend services and is going to pass through the backend security group.
- So instance will access RDS on port 3306, ElastiCache on 11211, and like that, Amazon MQ also. 5672. So in order to allow all those port numbers access, from the instance you need to allow the traffic on the backend security group. You can give specific rules also, which is much safer. Like you can say 3306 (this is for RDS), is allowed from the instance security group. You can say add rule, 11211 allowed from the instance security group. And wait. Yeah, one more rule. 5672 allowed from the instance security group.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/01a7b244-cc8c-4089-a9be-df6a3d9590e9)

- Vprofile backend security group rules we updated and it is now allowing traffic from the front end security group which is the instance security group.

### 127. Build & Deploy Artifact
- Okay, so we have everything ready. We have a database which is initialized also, we have ElastiCache cluster and we have a RabbitMQ instance. We have saved or I have saved all the information of it in the sticky note

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/9bcd4112-dec4-434c-b811-a8f8e8ee5256)

- And now we have the Beanstalk and it's in severe health state. That is obvious because we have changed the health check to slash login. And target group is detecting that slash login. But after we deploy our artifact, this will come back to normal.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/79ecfb3e-54ba-4182-8b45-78a89989cf1d)

- Okay, so we have to build and deploy the artifact and we have to make sure we mention all the backend services detail in application properties file. So, I'm going to create a folder in my computer somewhere, clone the source code there and we'll build the source code from there. You see the branch as main, we can use the same branch. There is also aws-refactor branch.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/230ef6c5-d1eb-4c78-9ce7-e489aa41dedd)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/d9eb824f-86c0-4c0b-9b36-318e9f26e83f)

- So, go to src, main, resources, application.properties file. Double click on that, open it. So, we will enter the MySQL, memcached, and RabbitMQ details. Starting from mysql, we have to remove this db01 from here and get the RDS endpoint, either from AWS console or from here, from your notes.
  
  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/3e0a2e2e-1954-48f7-aa64-380063717922)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/834fec72-d566-4cc0-94bb-c08fcca5a7f9)

- Open git bash from source code

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/f44b116c-df09-4f16-8267-974e434af26d)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/f486ecb9-4c03-47d9-ace1-045e738ab71c)

- Now, let's go to Beanstalk. Go to your Environment, find your environment, and then you should have an option to upload and deploy.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/dd8c3e77-64d6-41d8-9c36-025f568324f7)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/f09a7661-137c-4b41-8a51-28ec29fecefb)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/fdf8644e-e3df-4021-b50f-b04ef48d4147)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/fff2400f-dcb8-4b6e-a1d9-44eb70063fd6)

- Okay, so after waiting for like 10 to 15 minutes, everything became stable, but let's look at the events.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/c53f278d-c31e-45f6-82f2-d2df6b9b83ff)

- Okay, now the instance is in a good state and we can check the URL. There, we click on that.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/0576defd-905b-4a3c-bcea-6ac5f02c6c70)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/cbeba120-1429-4cef-875e-6a077cdf6055)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/2bb8df61-3041-47e5-871f-6e3e1a05f6e9)


### 128. Cloud front
- So your website data will be cached around the world. So you can serve a global audience easily, quickly. So once your application stack is up and running you can use a CDN network to distribute your content around the world. So AWS CloudFront or Amazon CloudFront is AWS CDN which you can use very easily.

- So let's say create CloudFront distribution.


  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/389cd62e-b1c5-4699-86f9-a5b2383b126c)

- You can directly select your S3 bucket or load balancer, which is served from CloudFront or you can give your domain name, vprofile.goophy.in.


  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/99a01cdf-932c-4897-b0d4-f06be6ed23cf)

- Origin path keep it as it is because our application automatically redirects to slash login. We don't need to mention that

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/c80c9cae-7bdd-4886-b3cb-006360d0a2b4)

- And come down, viewer protocol policy will keep HTTP, HTTPS if you want to serve both the traffic. Allowed HTTP method, I will select all, get, head, options, put, post, patch, delete and then let's come down.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/c8e7cf5c-fee9-477b-a41e-f8cfd0b3a7e7)

- And then let's come down. We'll keep all these things default settings. So this is about the pricing. If you want AWS to use all its edge location for the CloudFront here I'll show you 300 plus edge location. So if you select in CloudFront all edge location it'll distribute it in all the edge location or you can be specific if you're in North America and Europe, you can just select this, or not you as in your users.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/588419bf-88d2-4078-b9ef-388a87e1ee0f)

- Okay, let's come down. Alternate domain name. We are using an alternate domain name vprofile.goophy.in, that's mine. Put your domain over here, your CNAME. We are going with 443 HTTPS connection. So we need to select the certificate. So select the certificate, SSL certificate and security policy I'm going with TLSv1. Now for some reason if your CloudFront distribution is not working you try switching it back and forth among these different policies, TLSv1 definitely works for our vprofile project.

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/59cc8bbf-7143-42a2-8afa-7ede21f899fe)

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/27190872-ac3e-4864-b056-b9e6eb847b75)

- Reedit this step

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/ecbecc9e-0302-474f-a30c-bca37fc162dc)

### 129. Validate and Summarize
- Okay, so after some time, the status is enabled, it is deployed. So we can again access this URL on HTTPS protocol from our browser, but as a user, we won't be able to see the difference that it is coming, it is sold from the edge location of CloudFront,
or is it coming directly from the instance from that region. So to inspect that, I'm going to inspect the element while we access the website in our browser.


  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/fc123068-ae15-4ea0-b036-9feda5ad41de)

- So I'm opening Firefox here, just a different browser, so there's no caching in that browser yet. And I have opened the incognito mode of Firefox, just use an alternate browser. Don't use the same browser that you used previously to access your application.


  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/302540eb-3674-4cd8-95fa-7fd5ceacca75)

- So you can see via, you can see the CloudFront nearest distribution endpoint there. So our website is really getting now served from CloudFront distribution, and that was pretty amazing job. Now we have our entire stack, and we are using PaaS and SaaS services. We are not directly using EC2 instances, right?
- So if you have hosted zone in Amazon Route 53, you can even add the entry, instead of GoDaddy, you can add the entry over here, it's in the route 53 itself, or you can use both Route 53 for subdomain, and GoDaddy for the domain.
### Clean up
- Now let's fall back to our Amazon console. We need to do cleanup, we have lot many things.
- Starting from, let's delete CloudFront, so you can't directly delete it, first you to disable it, and it's gonna take some time to disable

  ![image](https://github.com/hieunguyen0202/DevOps-Training/assets/98166568/111ec882-ae41-4322-b1cc-ad3f261f640b)

- Let's go and start deleting other services. So RDS, let's delete our RDS instance.

  ![image](https://github.com/hieunguyen0202/Upload/blob/main/Annotation%202023-12-11%20081851.png)

  ![image](https://github.com/hieunguyen0202/Upload/blob/main/Annotation%202023-12-11%20081910.png)

  ![image](https://github.com/hieunguyen0202/Upload/blob/main/Annotation%202023-12-11%20081932.png)

- Let's go to ElastiCache, action

  ![image](https://github.com/hieunguyen0202/Upload/blob/main/Annotation%202023-12-11%20081959.png)

- Delete Amazon MQ.

  ![image](https://github.com/hieunguyen0202/Upload/blob/main/Annotation%202023-12-11%20082053.png)

- Let's go to Beanstalk, and this will also take a very long time to delete. Select it, first let's delete the environment.

  ![image](https://github.com/hieunguyen0202/Upload/blob/main/Annotation%202023-12-11%20082139.png)

  ![image](https://github.com/hieunguyen0202/Upload/blob/main/Annotation%202023-12-11%20082202.png)

- So last thing, make sure you delete your distribution.

  ![image](https://github.com/hieunguyen0202/Upload/blob/main/Annotation%202023-12-11%20082254.png)
