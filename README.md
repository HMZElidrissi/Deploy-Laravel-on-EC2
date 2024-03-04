# Deploying a Laravel Application on AWS EC2 using Laravel Sail with Docker
Here is a step by step guide to deploy a Laravel application on AWS EC2 using Laravel Sail with Docker. Laravel Sail is a light-weight command-line interface for interacting with Laravel's default Docker development environment. It is designed to provide a minimal configuration for Laravel apps that are using Docker.

## Step 1: Create an AWS EC2 instance
1. **Create an EC2 Instance**: Log in to your AWS Management Console, navigate to the EC2 Dashboard, and launch a new instance. Choose an Ubuntu Server as the Amazon Machine Image (AMI) because Laravel Sail runs in a Docker container, which is compatible with Linux environments.
2. **Configure Instance Details**: Select the instance type and configure instance details according to your needs.
3. **Configure Security Group**: Add rules to allow HTTP (port 80), HTTPS (port 443), and SSH (port 22) traffic.
4. **Launch Instance**: Review and launch your instance. Make sure to select a key pair for SSH access.

## Step 2: Connect to the EC2 instance
- **Connect to the EC2 Instance**: Use the SSH key pair to connect to the EC2 instance using the terminal or an SSH client.
```bash
ssh -i /path/to/your-key-pair.pem ubuntu@your-ec2-instance-ip
```

## Step 3: Install Docker and Docker Compose
1. **Update and Upgrade Packages**: Run the following commands to update and upgrade the package list on the EC2 instance.
```bash
sudo apt update
sudo apt upgrade
```
2. **Install Docker**: Run the following commands to install Docker on the EC2 instance.
```bash
sudo apt-get install -y docker.io
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ${USER}
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

## Step 4: Clone the Laravel Application
- **Clone the Laravel Application**: Clone your Laravel application from the repository to the EC2 instance using Git.
```bash
git clone https://github.com/HMZElidrissi/Deploy-Laravel-on-EC2.git
```
- if you need to clone a certain branch, you can use the following command:
```bash
git clone --branch <your-branch-name> https://github.com/HMZElidrissi/Deploy-Laravel-on-EC2.git
```

## Step 5: Install Composer and Dependencies
1. **Install Composer**: Run the following commands to install Composer on the EC2 instance.
```bash
sudo apt-get install composer
```
2. **Install Dependencies**: Navigate to the root directory of your Laravel application and run the following command to install the dependencies.
```bash
cd path/to/your/laravel-app
composer install
```
> if you need to install php8.3, you can use the following commands:
```bash
sudo apt -y install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get install php8.3
sudo apt-get install php8.3-cli php8.3-fpm php8.3-mysql php8.3-xml php8.3-mbstring php8.3-curl php8.3-zip php8.3-intl php8.3-gd
sudo update-alternatives --set php /usr/bin/php8.3
```

3. Install Sail: Run the following command to install Laravel Sail.
```bash
composer require laravel/sail --dev
php artisan sail:install
```

4. If you need to use an alias for the `sail` command, you can add the following line to your `.bashrc` file:
```bash
alias sail='bash vendor/bin/sail'
```

## Step 6: Run the Laravel Application with Sail
- **Run the Laravel Application**: Run the following command to start the Laravel application using Laravel Sail.
```bash
./vendor/bin/sail up -d
```

## Step 7: Access Your Application
- **Access Your Application**: Open a web browser and enter the public IP address of your EC2 instance to access your Laravel application.

---
- You may run into this error:

<img src="Screenshot from 2024-03-04 20-14-53.png" alt="error" width="800"/>

- To fix this error, you need just to grant write permissions to the storage directory:
```bash
sudo chmod -R 777 storage
sudo chmod o+w ./storage/ -R
```
> Don't forget to change the `APP_DEBUG` to `false` in the `.env` file 😉

---
This guide assumes a basic setup. Depending on your specific requirements (like using a database, queues, etc.), you might need to perform additional configuration steps.