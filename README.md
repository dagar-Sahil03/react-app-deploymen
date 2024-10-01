# Deploying a Basic React Application on Ubuntu EC2 Using Apache

## Table of Contents
1. [Step 1: Set Up Your EC2 Instance](#step-1-set-up-your-ec2-instance)
   - [Launch an EC2 Instance](#launch-an-ec2-instance)
   - [Connect to Your EC2 Instance](#connect-to-your-ec2-instance)
2. [Step 2: Install Node.js and Apache](#step-2-install-nodejs-and-apache)
3. [Step 3: Create a React Application](#step-3-create-a-react-application)
4. [Step 4: Configure Apache to Serve the React App](#step-4-configure-apache-to-serve-the-react-app)
5. [Step 5: Access Your React Application](#step-5-access-your-react-application)
6. [Conclusion](#conclusion)

## Step 1: Set Up Your EC2 Instance

### Launch an EC2 Instance
- Choose an Ubuntu AMI (e.g., Ubuntu 20.04).
- Select an instance type (e.g., t2.micro for free tier).
- Configure security groups to allow HTTP (port 80) and SSH (port 22) traffic.

### Connect to Your EC2 Instance
Open your terminal and connect to your EC2 instance:


ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-dns


##Step 2: Install Node.js and Apache
Update Packages

	sudo apt update
	sudo apt upgrade -y

Install Node.js and npm

	sudo apt install nodejs npm -y

Install Apache

	sudo apt install apache2 -y


##Step 3: Create a React Application

Install Create React App

	sudo npm install -g create-react-app

Create a New React App
Navigate to the home directory or a preferred location:

cd ~

npx create-react-app my-app

Build the React Application

Change into the project directory and build the app:

cd my-app
npm run build

##Step 4: Configure Apache to Serve the React App

Copy Build Files to Apache Directory
Copy the contents of the build directory to Apache's default root directory:

	sudo cp -r build/* /var/www/html/

Set Permissions
Set the proper permissions for Apache:

	sudo chown -R www-data:www-data /var/www/html

Configure Apache
(Optional) If you want to customize Apache settings, edit the default configuration:


	sudo nano /etc/apache2/sites-available/000-default.conf

Make sure it looks something like this:

apache
Copy code
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html

    <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted

        RewriteEngine On
        RewriteBase /
        RewriteRule ^index\.html$ - [L]
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule . /index.html [L]
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>


Enable the Rewrite Module (if using React Router)
If your React app uses routing, enable the rewrite module:

sudo a2enmod rewrite
Restart Apache
Restart Apache to apply the changes:

sudo systemctl restart apache2


##Step 5: Access Your React Application

Get Your EC2 Public IP
You can find your public IP in the EC2 console or by running:

	curl http://169.254.169.254/latest/meta-data/public-ipv4

Open a Browser

Navigate to http://your-ec2-public-ip. Your React app should now be visible!

Conclusion
You’ve successfully deployed a basic React application on an Ubuntu EC2 server using Apache. You can share the public IP address, and anyone can access the application from their browser.