# Title

This case study is intended to set up and deploy webapp on centos machine.

# Technologies to be used.

Frontend:  HTML

Backend: Django

Database: SQL

Cloud Platform: AWS

# Infrastructure Setup

Networking 

Step 1: Launch VPC with name myvpc with cidr : 10.0.0.0/16 

Step 2: Launch Internet Gateway named as myigw 

Step 3: Launch Subnet with name mysubnet with cidr 10.0.0.0/24 

Step 4: Launch Route Table with myrtb 

Step 5: Follow the below steps to configure internet connection.(otherwise we can’t connect to resources which resides in myvpc from our local network) 

attach myigw to myvpc

add myigw to routes in myrtb(destination 0.0.0.0/0, target myigw)

associate mysubnet in myrtb 

Security

Step 1: Security group with name mysg(add inbound rule to open traffic for all ports)

Step 2: Create keypair with name mykp(select private key file format as a .pem)

Computing

Step 1: Launch 1 EC2 instance(centos7 image) with name mywebserver(Auto-assign public IP : Enable)

# Install and configure required software packages. 

Step 1: Connect to the above launched instance

$ssh -i <<mykp>> centos@<<PUBLIC_IP>>

Step 2: Install git 

$sudo yum install git -y

Step 3: Install pip package manager

$sudo yum install python3-pip -y

Step 4: Install django package
$sudo pip3 install django

$sudo vi /usr/local/lib64/python3.6/site-packages/django/db/backends/sqlite3/base.py  (line 67   replace > with ==)
Step 4: Install gunicorn server

$sudo pip3 install gunicorn

# Deployment

$git clone  http://github.com/akhilvaka2/webapp.git 

$cd webap

$python3 manage.py makemigrations

$python3 manage.py migrate

$gunicorn main.wsgi --bind 0.0.0.0:8000

# Testing

$curl localhost:8000

$curl 127.0.0.1:8000

$curl <PRIVATE_IP>:8000

$curl <PUBLIC_IP>:8000

$http://<PUBLIC_IP>/8000
