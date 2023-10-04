# WordPress_MySQL_-Deployment
## ❖  Task: Deploy application in monolithic and microservices architecture

## ❖ For monolithic

1. In the EC2 Dashboard, click the "Launch Instance" button to start the instance creation process.

  2.  Choose an Amazon Machine Image (AMI)

       In the "Application and OS Images" :

- Choose an Ubuntu AMI.
- Select the Ubuntu AMI that matches your region and desired version (e.g., Ubuntu Server 22.04 LTS).

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/db333ce9-18b8-4523-8fa8-bf106f5a7158/84fd7890-0abf-46cb-90f4-6a280ffb7e94/Untitled.png)

1. **Choose an Instance Type**

       In the "Choose an Instance Type" step:

- Select the instance type as "t2.micro"

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/db333ce9-18b8-4523-8fa8-bf106f5a7158/b6681356-f4a1-4bda-944e-7340e5ec367c/Untitled.png)

1. In the key pair section , create a key pair(Keypair Type: RSA, File format:.pem) and download the key pair file (e.g., **`your-key-pair.pem`**) or choose and existing key pair. *This file is essential for SSH access to your instance* 
2. Under Network Settings, **Configure Security Group**

       Allow inbound traffic:

- HTTP (Port 80) from 0.0.0.0/0
- HTTPS (Port 80) from 0.0.0.0/0
- SSH (Port 22) from anywhere or a trusted IP range.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/db333ce9-18b8-4523-8fa8-bf106f5a7158/cc56448c-468a-4271-856e-d0b551024a8c/Untitled.png)

1. You can leave remaining settings at their default values(ie Configure storage and Advanced details)
2. **Review and Launch** 
    - Review your instance configuration to ensure it's correct.
    - Click the "Launch" button.

  8. Access Your Ubuntu EC2 Instance

- Once your instance is running (it may take a few minutes to initialize), select it in the list of instances.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/db333ce9-18b8-4523-8fa8-bf106f5a7158/665fdb7a-09d4-4fc9-a504-8bcd0011862e/Untitled.png)

- Click the "Connect" button at the top.
- Follow the instructions provided to SSH into your Ubuntu instance using the key pair you downloaded earlier. eg: **`ssh -i <your-key-pair.pem> ubuntu@<instance_public_IP>`**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/db333ce9-18b8-4523-8fa8-bf106f5a7158/dbfa8086-b281-46a0-a7e1-336a8edaa8c3/Untitled.png)

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
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/db333ce9-18b8-4523-8fa8-bf106f5a7158/c728c397-efe4-4893-b6da-61984b6b5875/Untitled.png)
    
3. You will get below screen and enter the required information, click on Install WordPress

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/db333ce9-18b8-4523-8fa8-bf106f5a7158/403119d2-0e1d-454c-9527-37d8af3789f8/Untitled.png)

1. Log into WordPress.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/db333ce9-18b8-4523-8fa8-bf106f5a7158/17d0c3ee-7a5b-4093-b5c5-7469dcc4cb2a/Untitled.png)
    

————————————————————————————————————————————

## ❖ For **Microservices Architecture**

1. **Launch two EC2 instances: Choose the `t2-micro` instance type and `Ubuntu` AMI for both. Make sure to create a new key pair and download it to your local machine.**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/db333ce9-18b8-4523-8fa8-bf106f5a7158/641cc0d3-9cd2-4bdf-a5d0-a06ea9289e9d/Untitled.png)

1. **Create two Security Groups: One for the WordPress instance and one for the MySQL instance. For the WordPress instance, allow inbound traffic on ports 22, 80, and 443. For the MySQL instance, allow inbound traffic on port 3306 from the WordPress instance’s security group.**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/db333ce9-18b8-4523-8fa8-bf106f5a7158/f5880d82-cf23-4db5-8903-7a11a4af8d5d/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/db333ce9-18b8-4523-8fa8-bf106f5a7158/ffa69111-2213-4d4b-ba6c-e8041d0e5c77/Untitled.png)

1. **Install WordPress on one instance and MySQL on the other:**
    - SSH into your WordPress instance and install Apache and PHP as described above.
    - SSH into your MySQL instance and install MySQL as described above.
    - Configure WordPress to connect to the MySQL database on the other instance.
2. **Create a welcome page: Log into your WordPress site and create a new page that will serve as the homepage.** Create a welcome page. Pages>new>edit and publish it
