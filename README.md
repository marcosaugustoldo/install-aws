# Tutorial: Installing an EC2 Instance with Amazon Linux 2023 and Configuring IAM Role and Security Group

**Index**

1. [Introduction](#introduction)
2. [Creating an IAM Role for SSM](#creating-an-iam-role-for-ssm)
3. [Creating a Security Group](#creating-a-security-group)
4. [Creating an EC2 Instance](#creating-an-ec2-instance)
5. [Configuring the EC2 Instance](#configuring-the-ec2-instance)
6. [Verifying the Connection](#verifying-the-connection)

**1. Introduction**

In this tutorial, we will create an EC2 instance with Amazon Linux 2023, configure an IAM role to allow access to SSM (Systems Manager), and create a security group to enable ports 8080 and 443.

**2. Creating an IAM Role for SSM**

1. Access the AWS console and navigate to the IAM service.
2. Click on "Roles" and select "Create role".
3. Select "EC2" as the service that the role will be used for and click on "Next: Review".
4. In "Review", select "AmazonSSMManagedInstanceCore" as the policy and click on "Create role".
5. Note the name of the role created, as it will be needed later.

**3. Creating a Security Group**

1. Access the AWS console and navigate to the EC2 service.
2. Click on "Security Groups" and select "Create security group".
3. In "Create security group", add a rule to allow traffic on port 8080 and click on "Add rule".
4. Add another rule to allow traffic on port 443 and click on "Add rule".
5. Select "Create security group" and note the ID of the security group created.

**4. Creating an EC2 Instance**

1. Access the AWS console and navigate to the EC2 service.
2. Click on "Launch Instance" and select "Amazon Linux 2023" as the instance image.
3. Select the instance type "t2.micro" (which is free) and click on "Next: Configure Instance Details".
4. In "Configure Instance Details", select the IAM role created earlier and the security group created earlier.
5. Click on "Next: Add Storage" and select "8 GB" as the root volume size.
6. Click on "Next: Add Tags" and add a tag with the name "Name" and the value "My EC2".
7. Click on "Next: Configure Security Group" and select the security group created earlier.
8. Click on "Next: Review and Launch" and select "Launch".
9. In "Configure Instance Details", click on "Advanced details" and select "As text".
10. Paste the following script in the text field:
```bash
#!/bin/bash

#Install Docker and Git
sudo yum update -y
sudo yum install git -y
sudo yum install docker -y
sudo usermod -a -G docker ec2-user
sudo usermod -a -G docker ssm-user
id ec2-user ssm-user
sudo newgrp docker

#Enable Docker
sudo systemctl enable docker.service
sudo systemctl start docker.service

#Install Docker Compose 2
sudo mkdir -p /usr/local/lib/docker/cli-plugins
sudo curl -SL https://github.com/docker/compose/releases/download/v2.23.3/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose

#Add swap
sudo dd if=/dev/zero of=/swapfile bs=128M count=32
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo echo "/swapfile swap swap defaults 0 0" >> /etc/fstab

#Install Node and npm
curl -fsSL https://rpm.nodesource.com/setup_21.x | sudo bash -
sudo yum install -y nodejs
```
11. Click on "Launch" to create the instance.

**5. Configuring the EC2 Instance**

1. Access the AWS console and navigate to the EC2 service.
2. Select the instance created earlier and click on "Actions" and select "Instance settings".
3. In "Instance settings", verify that the IAM role and security group are configured correctly.

**6. Verifying the Connection**

1. Access the AWS console and navigate to the EC2 service.
2. Select the instance created earlier and click on "Connect to instance".
3. Select "Session Manager" and click on "Connect".
4. Verify that the connection was established successfully.

**Hotkeys**

* W: Go back to the beginning of the tutorial
* A: Verify the configuration of the EC2 instance
* S: Verify the configuration of the security group
* D: Verify the connection to the EC2 instance
