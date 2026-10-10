# Setting up EC2 for single Instance in AWS Console

This guide will walk you through setting up the AWS Console for managing your cloud resources.

## Prerequisites
- An AWS account
- Access to the AWS Management Console
- Basic understanding of AWS services

## Steps
1. Sign in to the AWS Management Console
2. Search for "EC2" in the AWS Management Console search bar and select it from the results.
3. In the EC2 Dashboard, click on "Instances" in the left-hand menu.
4. Click on the "Launch Instance" button to create a new EC2 instance.
5. Follow the prompts to configure your instance, including selecting an Amazon Machine Image (AMI), instance type, and key pair.
6. Review your settings and click "Launch" to start your EC2 instance.
7. Once the instance is running, you can connect to it using the key pair you selected during the setup process.

8. Verify that your instance is running and accessible by checking its status in the EC2 Dashboard.
9. Change permissions for your key pair file to ensure it is secure.
```bash
chmod 400 /path/to/your-key-pair.pem
```
10. Connect to your EC2 instance using SSH.
```bash
ssh -i /path/to/your-key-pair.pem ec2-user@your-ec2-instance-public-dns
```
11. You should now be connected to your EC2 instance and can start managing it as needed.