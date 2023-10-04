# WordPress_MySQL_-Deployment
## ❖  Task: Deploy application in monolithic and microservices architecture

## ❖ For monolithic

1. In the EC2 Dashboard, click the "Launch Instance" button to start the instance creation process.

  2.  Choose an Amazon Machine Image (AMI)

       In the "Application and OS Images" :

- Choose an Ubuntu AMI.
- Select the Ubuntu AMI that matches your region and desired version (e.g., Ubuntu Server 22.04 LTS).

![Untitled](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/4b460e19-3de4-4997-8694-bacc9e7706a7)


1. **Choose an Instance Type**

       In the "Choose an Instance Type" step:

- Select the instance type as "t2.micro"

![Untitled 1](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/2fe87ff2-9cb5-427a-a90b-f55a98f52a68)


1. In the key pair section , create a key pair(Keypair Type: RSA, File format:.pem) and download the key pair file (e.g., **`your-key-pair.pem`**) or choose and existing key pair. *This file is essential for SSH access to your instance* 
2. Under Network Settings, **Configure Security Group**

       Allow inbound traffic:

- HTTP (Port 80) from 0.0.0.0/0
- HTTPS (Port 80) from 0.0.0.0/0
- SSH (Port 22) from anywhere or a trusted IP range.

![Untitled 2](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/9acf1576-13c8-46a7-8953-6346ac307fbb)

1. You can leave remaining settings at their default values(ie Configure storage and Advanced details)
2. **Review and Launch** 
    - Review your instance configuration to ensure it's correct.
    - Click the "Launch" button.

  8. Access Your Ubuntu EC2 Instance

- Once your instance is running (it may take a few minutes to initialize), select it in the list of instances.


![Untitled 3](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/3a514c5f-2c13-4bad-9340-3264b8fe9e6c)

- Click the "Connect" button at the top.
- Follow the instructions provided to SSH into your Ubuntu instance using the key pair you downloaded earlier. eg: **`ssh -i <your-key-pair.pem> ubuntu@<instance_public_IP>`**

![Untitled 4](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/2e0c123d-d8ec-4c73-8096-339f67652b0e)


1. Install MySQL and create a Database for WordPress:
- SSH into the instance:
    
    ```bash
    ssh -i <your-key.pem> ubuntu@<instance-public-ip>
    
    ```
    
- Update the package list and install MySQL Server:
    
    ```bash
    sudo apt update
    sudo apt install mysql-server -y
    ```
    
- Secure your MySQL installation by running:
    
    ```bash
    sudo mysql_secure_installation
    ```
    
- Follow the prompts to set a root password and answer the security questions.
- Create a MySQL database for WordPress:

```bash
mysql -u root -p
```

- Inside MySQL, run: To create a user with permissions for the database

```sql
CREATE DATABASE wordpress;
#set a stong password in <enter-your-password>
CREATE USER 'wordpress'@'localhost' IDENTIFIED BY '<enter-your-password>';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

1.  Install WordPress
- Install Apache with **`sudo apt-get install apache2 -y`**
- Install PHP with **`sudo apt-get install php libapache2-mod-php php-mysql -y`**
- Download and extract the latest WordPress: `wget https://wordpress.org/latest.tar.gz`
extract it `tar -xvzf latest.tar.gz`
- Move the WordPress files to the Apache web directory: `sudo mv wordpress/* /var/www/html/`
1. Configure WordPress database details by editing the WordPress configuration file located at /var/www/html/wp-config.php(rename the wp-sample.php with wp-config.php) and add the database details
2. Access your WordPress site URL (http://<instance-public-ip>/wp admin or access by the instance public ip by removing the index.html file from `/var/www/html/` using `sudo rm -rf index.html` ) and you get following the setup wizard. Enter the details and click on submit
    
  
    ![Untitled 5](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/fe127ea7-f5ab-4c92-8cab-ba68c33b5d41)

3. You will get below screen and enter the required information, click on Install WordPress


![Untitled 6](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/29fe3d6d-ba97-4bdd-8a98-f1908b04a4e6)

1. Log into WordPress.
    
   
    ![Untitled 7](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/9b87cea2-bb54-46e8-88b8-62cceca7efde)


————————————————————————————————————————————

## ❖ For **Microservices Architecture**

1. **Launch two EC2 instances: Choose the `t2-micro` instance type and `Ubuntu` AMI for both. Make sure to create a new key pair and download it to your local machine.**

![Untitled 8](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/d5691aa2-d8b3-48fa-b03e-f88b992f79f0)


1. **Create two Security Groups: One for the WordPress instance and one for the MySQL instance. For the WordPress instance, allow inbound traffic on ports 22, 80, and 443. For the MySQL instance, allow inbound traffic on port 3306 from the WordPress instance’s security group.**


![Untitled 9](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/7062548a-bc0a-4a0c-a77e-397ce1ea0a7c)

![Untitled 10](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/013dfd6a-6aca-4631-9e40-3a8bcc1dec78)


1. **Install WordPress on one instance and MySQL on the other:**
    - SSH into your WordPress instance and install Apache and PHP as described above.
    - SSH into your MySQL instance and install MySQL as described above.
    - Configure WordPress to connect to the MySQL database on the other instance.
2. **Create a welcome page: Log into your WordPress site and create a new page that will serve as the homepage.** Create a welcome page. Pages>new>edit and publish it
   
![Untitled 11](https://github.com/VishAstra/WordPress_MySQL_-Deployment/assets/122463168/2a07b8f2-8e60-4acd-9254-ab6089d582db)
