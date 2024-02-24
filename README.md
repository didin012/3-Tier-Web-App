# Three Tier Web App Architecture
## Infrastructure
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/740cb89a-0a90-47c6-851d-e041b0ab480b)

## Definition
Three-tier architecture divides software applications into three logical and physical computing tiers: the application tier, which processes data, the data tier, which stores and manages application-related data, and the presentation tier, which is the user interface. 

### Presentation Tier
The application's user interface and communication layer, or presentation tier, is where users interact with the program. Information display and data collection from the user are its primary goals. This top-level tier can operate, for instance, as a desktop application, graphical user interface (GUI), or web browser. JavaScript, HTML, and CSS are typically used in the development of web presentation tiers. Depending on the platform, desktop apps can be created in a wide range of languages.
### Logic Tier
The core of the application is the application tier, sometimes referred to as the middle tier or logic tier. This tier uses business logic, or a particular set of business rules, to process data gathered in the presentation tier, often cross-referencing it with data from the data tier. Data in the data tier may also be added, removed, or modified by the application tier. 

It is commonly written in Python, Java, Perl, PHP, or Ruby, the application tier interfaces with the data tier through API calls.  On this project, we will use PHP.
### Database Tier
The data tier—also referred to as the database tier, data access tier, or back-end—is where the application's processed data is kept and organized. This could be a NoSQL database server like Cassandra, CouchDB, or MongoDB, or it could be a relational database management system like PostgreSQL, MySQL, MariaDB, Oracle, DB2, Informix, or Microsoft SQL Server. 

All communication in a three-tier application occurs at the application tier. Direct communication between the data tier and the presentation tier is not possible.
### Benefits of three-tier architecture
**Faster development:** An company can launch the application more quickly and programmers can utilize the best and most recent languages and tools for each layer since each tier can be developed concurrently by various teams.

**Enhanced scalability:** Each tier has the flexibility to scale separately from the others as required.

**Enhanced dependability:** The likelihood of an interruption affecting one tier's availability or performance is reduced for the other levels.

**Enhanced security:** Since there is no direct communication between the presentation and data tiers, a well-designed application tier can act as an internal firewall to stop SQL injections and other dangerous vulnerabilities.

## On this Project you will be able to

1. Create and assign accordingly the VPC, subnets, NAT Gateway, Internet Gateway, Elastic IP, and Security groups into the EC2 instances for network isolation.
2. Configure instance specifications such as instance type, AMI selection, networking, and storage options.
3. Understand how to SSH into EC2 instances using the public IP address provided by AWS.
4. Learn about the syntax and options for the SSH command (e.g., specifying the private key file, username, and hostname).
5. Be able to connect to other EC2 instances (app server) using front facing instance via NAT Gateway
6. Install PHP and necessary PHP extensions using the package manager available on the chosen Linux distribution.
7. Learn to deploy web servers like Apache HTTP Server or PHP on Amazon Linux (AMI) EC2 instances.
8. Access the PHP file through a web browser to verify that PHP is executing correctly.
9. Configure load balancers such as Application Load Balancing (ALB) to distribute incoming traffic among multiple web servers for high availability and scalability.
6. Configuring Sticky Sessions in Application Load Balancers

## First Step: Configuring Virtual Private Cloud and its Components
1. Create a VPC

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/dfc86661-fb1d-4220-9b95-8a35a39014a5)

2.	Under the Subnet section, choose Create Subnet then follow the specification on the table below

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/3a872802-d36d-4100-839c-9ad10ec9c5ff)

3.	Create a Routing Table then associate all the related subnet on the route table

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/daa3fe78-5e8f-4e1a-9f5c-d5557dbb301a)

4.	Create an **Internet Gateway** and attach it to your VPC
5.	Go to **NAT Gateway** and create one. Pinpoint it to the ```web-public-subnet-1``` then connectivity type is Public. Allocate an Elastic IP for this one. 

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/c77dd6a7-76ae-4a5c-8fb8-3cc797a7b321)

6.	Go back to your Route Table and configure the routes by clicking **Edit Routes** on each Route tables

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/a249221f-5235-450e-a34f-25c791ec1b2e)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/f959bd8c-2f22-4cb1-8e18-a063895348bb)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/ca42746c-37d0-4291-96eb-427a7fbcdca1)

## Launching EC2 Instances
1.	Click on **Launch Instance** in the EC2 dashboard then follow the following configurations in the images below. This will be our jump server, where we will use to access our app server later.

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/752acae2-ee59-4d3d-ad50-971206cdfbed)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/6417f2b1-8202-410d-9d4c-442a4c2ce318)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/110a31a1-9574-4bea-8022-d5c790f32dc3)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/297b7e7c-7c3b-4a9f-be5a-89671894c8d7)

2. You should have your own keypair.pem, keep it somewhere in your local directory (mine is myubuntukey.pem).
3. Create the PHP instances (app instance) then follow these input details below in the pictures. Remember to select the right VPC, subnet (```app-private-subnet-1```), and create its own Security Group.

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/d7d82431-a1ab-4d07-85e5-5cee1c051ca9)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/36ec5b6b-35af-4e05-891c-8e4439cf0069)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/8f37f93a-c9b2-4af6-955b-814969ea2fda)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/3928475f-7d54-40c9-b54b-b9734f88bfa5)

3.	Create another PHP app server then name it into PHP app server 2. You should select the ```app-private-subnet-2``` for its Subnet. Associate the same Security Group used in ```app-private-subnet-1```.

 ## Accessing our Public Instance via SSH

1. Since you have downloaded your own keypair, you should be able to store it in your local directory and in CLI, point it to the directory where it stored and type in this<br>
```$ chmod 400 <your_keypair_name>.pem```<br>
2. IMPORTANT!! Copy first this keypair to your public instance. <br>
```$ sudo scp -I <your_keypair_name>.pem <your_keypair_name>.pem ec2-user@<elastic_ip_address>:``` <br>
   Use SSH to access your instance using this command. Elastic IP address can be seen on the Jump Server assigned IPv4 Address<br>
```$ ssh -i <your_keypair_name>.pem ec2-user@<elastic_ip_address>```<br>
4.	Inside the instance run this command again to enable file privileges<br>
```$ chmod 400 <your_keypair_name>.pem```

## Accessing our Private Instance via SSH Inside the Public Instance

1.	Inside your Jump server instance, access private instance using SSH commands. You can see the PHP app server IPv4 on the PHP app server 1 EC2 instance IPv4<br>
```$ ssh -i <your_keypair_name>.pem ec2-user@<php_app_server_IPv4>```<br>
2.	You now have access to your private instance via NAT gateway

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/659d301b-b50e-448f-ad39-79da0dc07630)

3.	Install LAMP server inside this app instance. All of these commands are under this AWS documentation. https://docs.aws.amazon.com/linux/al2023/ug/ec2-lamp-amazon-linux-2023.html<br>
```$ sudo dnf update -y```<br>
```$ sudo dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel```<br>
```$ sudo dnf install mariadb105-server```<br>
```$ sudo systemctl start httpd```<br>
```$ sudo systemctl enable httpd```<br>
```$ sudo systemctl is-enabled httpd```<br>

4.	Give appropriate permission to ec2-user and follow further for the following commands<br>
```$ sudo usermod -a -G apache ec2-user```<br>
```$ sudo chown -R ec2-user:apache /var/www```<br>
```$ sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;```<br>
```$ find /var/www -type f -exec sudo chmod 0664 {} \;```<br>

5.	For sample application, install PHP MyAdmin<br>
```$ sudo dnf install php-mbstring php-xml -y```<br>
```$ sudo systemctl restart httpd```<br>
```$ sudo systemctl restart php-fpm```<br>
```$ cd /var/www/html```<br>
```$ wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz```<br>
```$ mkdir phpMyAdmin && tar -xvzf phpMyAdmin-latest-all-languages.tar.gz -C phpMyAdmin --strip-components 1```<br>
```$ rm phpMyAdmin-latest-all-languages.tar.gz```<br>
```$ sudo systemctl start mariadb```<br>
6. Create a sample HTML server<br>
```$ echo “PHP Server 1” > index.html```<br>
7.	**NOTE!!!** Do these commands as well on ```PHP Server 2``` then create another ```index.html``` file inside the ```/var/www/html/``` directory<br>
```$ echo “PHP Server 2” > index.html```

## Create an Application Load Balancer

1. Go to EC2 Management Console then into Load Balancer and **Create Load Balancer**. Choose Application Load Balancer. You should create its own Security Group. For

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/3832bf55-348d-4d42-9c31-577d05180c50)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/b15b0083-1f0c-49a8-b941-9a4ed19ad172)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/625422e3-adfc-40cd-ac63-7c57f14bd88a)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/a3f71c5d-f792-4f18-8427-6f3d27d09b44)

2.	For creation of the Target groups follow this details

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/c1bb6f99-0c9b-42f1-ac03-82c635e7a5b8)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/2de42254-fa44-4c45-830b-3df8aa700d79)

3. Lastly, the Registered Targetsm, select the app server instances

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/bc2037cc-1a70-41bc-9e76-9644f3fdcac9)

## Going Back to EC2 instances Security Groups

1.	Allow the ALB-sg to the App server security group. This will establish the connectivity between ALB and app server instances

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/2e23c30a-aae9-4660-81b6-58a66460c4cc)

2.	Now check the server on the browser using the DNS name of your Load Balancer. Add ```/index.html``` after the link

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/d0081c1f-b071-45ae-a345-57136d3fb321)

## Creating RDS for Database Tier

1.	Proceed on **Create DB Subnet Group**, select the VPC you created, db subnets, correct availability zones.

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/1d19c29b-252e-4dc7-84cc-b62ba71225b0)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/2261addf-edbb-4fbf-a118-6fac94c42698)

2.	Create an DB instance on RDS **Create Database**. Remember your Master username and Master password. Also, create a Security Group for this database

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/bb2a2710-0f06-45b2-9846-e39fb350529a)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/4fe8a7ea-3fa7-43af-bfd0-14586800725c)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/de8f2081-1405-472f-a6ea-ba4f9969277b)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/b6fb2085-43a4-47bf-9faa-86d4f0d5733a)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/379c2128-d15f-47a9-b236-eb424e0f5fb3)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/63ab401a-8836-4c00-8cd0-6e298346edb4)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/7a7a6a2e-144f-4eaa-a711-33767e509556)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/03a5b5ac-1b48-47e7-83bb-3af892adcc1d)

3.	We need to enable the connectivity between app server and db server. Go to security group of the DB. Delete the rule on the first row

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/dce4ff9c-0954-4008-ac78-e0ce86ea236a)


4.	Inside the PHP app server 1 instance CLI, go to ```/var/www/html/phpMyAdmin``` directory then rename the ```config.sample.inc.php``` into ```config.inc.php```

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/d585f57e-f548-4f10-b157-544427fa3b39)

5.	Open the ```config.inc.php``` using ```vim``` command then change the localhost on ```$cfg[‘Servers’][$i][‘host’] = ~ ``` into the copied endpoint link of the DB

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/3d6bdfa9-c4cf-4f56-bad8-853a92c16255)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/a3e87648-d557-462a-b1d6-dcf8acf6b3fa)

Save this file by pressing ```esc``` and type ```:wq```, this means Write Quit

6.	**NOTE! You should do this as well on the PHP app server 2 !!**

## Enable Sticky Sessions

1.	Go back to **Target Groups** and go to **Attributes** then **Edit**

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/50332443-221f-4721-9699-a473729d90a9)

2.	Inside the target group attributes, enable the Stickiness. This will make the request flow in one server. Incase of failure it will be redirected to the other server.

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/4dfffb50-8f6f-459c-a3ea-54cc5966055e)

## Running our phpMyAdmin server on the Browser

1.	Open up any browser then copy paste the link of our **Application Load Balancer** then add ```/phpMyAdmin``` at the end

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/923ac635-a78e-435e-80a4-3f89433f383e)

2.	Enter up your credentials based on what you have typed in during creating the RDS instance. This will give you access to your database layer.

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/d1b92fc5-6d78-4244-a3bd-664d2afa3373)

### You now have access to your DB through PHP app server!

## Cleaning Up

1.	Start with deleting the DB instance

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/ac5d0b8b-069c-4faf-8cc3-c5104974bbb3)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/54965b02-d3dc-4a26-b0cd-96a0935b1257)

2.	Delete Application Load Balancer

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/f9179078-45cd-47d5-b1b4-2e9ba5bcebac)
![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/723d7e7b-5a50-4549-91c4-45d10eca50b6)

3.	Terminate all EC2 instances used in the project

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/e7eb971f-719b-4e58-992c-6762395f3318)

4.	Delete all used Security Groups

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/51b71c75-c479-453e-8602-57fdcc6720f2)

5. Release your Elastic IP **(AWS will bill you on this if you forgot to remove)**

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/c0c421ad-c93a-4389-81df-723fd929857e)

6.	Delete NAT Gateway

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/46a74548-6c8f-4f90-9159-fc468797f1c7)

7.	Delete all used subnets

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/f20990c5-fa2f-438b-adc2-186e89b3a7a8)

8.	Delete Route Tables

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/23af1b3b-e10a-46e3-84b5-7a4f72d0495b)

9.	Detach IGW on your VPC

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/4cc423c8-cadc-4d7e-96a1-a8f7e51c2688)

10. Delete the VPC used in the project.

![image](https://github.com/didin012/Three-Tier-Web-App-Architecture/assets/104528282/f9149fd2-fe36-4f30-8821-a39f401af355)

