# EC2 Website Deployment Ansible Playbook

## Overview
This Ansible playbook automates the process of creating an EC2 instance, configuring security groups, and deploying a simple website with Nginx and SSL.

## Prerequisites
- Ansible installed
- AWS CLI configured with appropriate credentials
- SSH key pair for EC2 access

### Stage 1: Launch EC2 Instance
#### Security Group Configuration
- Creates a security group named `Ansible-sg`
- Opens ports:
  - 22 (SSH)
  - 80 (HTTP)
  - 443 (HTTPS)
  - 8080 (Optional web service port)
- Allows all outbound traffic

#### EC2 Instance Launch
- Uses `t2.micro` instance type
- Selects Ubuntu AMI
- Assigns public IP
- Uses specified SSH key pair
- Waits for instance to be fully running

### Stage 2: Instance Configuration
#### System Preparation
- Updates and upgrades system packages
- Installs required packages:
  - Nginx
  - OpenSSL
  - Python3

#### Website Setup
- Creates directories:
  - `/var/www/task1`
  - `/var/www/task2`
  - `/etc/ssl/private`
  - `/etc/ssl/certs`

- Generates two simple HTML pages:
  1. `/var/www/task1/index.html`
  2. `/var/www/task2/index.html`

#### SSL Configuration
- Generates self-signed SSL certificate
- Configures Nginx for SSL; copy nginx_ssl_config file from host instance to launched instance
- Sets up virtual hosting for two websites

#### SSH Configuration
- Generates a new SSH key pair
- Adds the public key to `authorized_keys`

## Nginx Configuration Details
- Listens on HTTP (80) and HTTPS (443)
- Uses self-signed certificate
- Serves two websites:
  - `/page1` serves content from `/var/www/task1`
  - `/page2` serves content from `/var/www/task2`

## Environment Variables
- `VPC_ID`: Required for security group creation
- Ensure this is set before running the playbook

## Usage
```bash
# Set VPC ID
export VPC_ID=your-vpc-id

# Run the playbook
ansible-playbook ec2_website_deployment.yml
```