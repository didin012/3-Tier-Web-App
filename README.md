# 3-Tier-Web-App

## Virtual Private Cloud
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






