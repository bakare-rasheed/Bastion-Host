# Setting Up a Secure VPC Infrastructure with Bastion Host Access


- Create a VPC:

In the AWS Management Console, navigate to the VPC Dashboard.
Click "Create VPC" and configure the VPC settings, including IP range (CIDR block), tenancy, and any additional settings.

![rasheed-vpc-llll](https://github.com/bakare-rasheed/Ansible-Projects/assets/114327344/b2558343-3091-4018-879c-0f93283fd6e6)


- Create Subnets:

Create two subnets within the VPC: one public subnet and one private subnet.
Public Subnet: This subnet should have a route to an Internet Gateway (IGW). Instances launched in this subnet will have public IP addresses and direct internet access.
Private Subnet: This subnet should have its default route pointed to a Network Address Translation (NAT) Gateway or NAT instance in the public subnet. This allows instances in the private subnet to access the internet for updates while preventing inbound traffic from the internet.

![rasheed-subnets](https://github.com/bakare-rasheed/Ansible-Projects/assets/114327344/b287a9ba-f05e-4bdc-8719-cabac7ac1efa)

![rasheed-RTables](https://github.com/bakare-rasheed/Ansible-Projects/assets/114327344/6ed530f7-7dfe-4ba2-be29-af57829a728d)

- Create a Security Group for the Bastion Host:

Create a security group for the bastion host instance in the public subnet.
Configure inbound rules to allow SSH traffic (port 22) from your IP address or a defined range.

 - Launch the Bastion Host:

Launch an EC2 instance in the public subnet (bastion host) 
Assign the security group created for the bastion host to the instance.
 
 - Create a Security Group for the Private Instance:

Create a security group for the instance in the private subnet.
Configure inbound rules to allow incoming SSH traffic (port 22) from the security group of the bastion host.

 - Launch the Private Instance:

Launch another EC2 instance in the private subnet (target instance) 
Assign the security group created for the private instance to the instance.
 
 - Configure Route Tables:

Create two route tables: one for the public subnet and one for the private subnet.
Associate the public subnet with the route table that has a route to the Internet Gateway.
Associate the private subnet with the route table that has a route to the NAT Gateway or NAT instance.

![rasheed-IGW](https://github.com/bakare-rasheed/Ansible-Projects/assets/114327344/3abc6b41-f6d6-478d-823a-0aec99fd6cf4)

![rasheed-IGWA](https://github.com/bakare-rasheed/Ansible-Projects/assets/114327344/99eada02-5a14-4d4b-b8c5-ef9b38753658)


![rasheed-RTables](https://github.com/bakare-rasheed/Ansible-Projects/assets/114327344/6ed530f7-7dfe-4ba2-be29-af57829a728d)


![rasheed-nat](https://github.com/bakare-rasheed/Ansible-Projects/assets/114327344/cb783833-bfda-43b1-b921-45c5c2d9421c)

![rasheed-nat2](https://github.com/bakare-rasheed/Ansible-Projects/assets/114327344/7eab0cfc-88cf-4017-a261-da58d7159f91)


- Adding of Keypair

To enable SSH into the private instance, you must add the private instance's key pair intothe public instance.
In this case, do `touc rasheed.pem` and copy-paste the key pair. 

 - Open your terminal and use the ssh command to connect to the bastion host using `ssh -A ec2-user@bastion-ip
 - Connect to Target Instance from Bastion:

![rasheed-pub-ssh](https://github.com/bakare-rasheed/Ansible-Projects/assets/114327344/0ffd814d-4c91-4384-8b5a-abb1e532da0d)

Once connected to the bastion host, use the ssh command again to connect to the instance in the private subnet using its private IP.
`ssh -A ec2-user@private-instance-ip

![rasheed-ssh-add](https://github.com/bakare-rasheed/Ansible-Projects/assets/114327344/1d5407ad-d52f-4b86-944a-a3cbf7418b89)

![rasheed-ping](https://github.com/bakare-rasheed/Ansible-Projects/assets/114327344/976f219d-1ecf-452b-a40e-55bca7020574)


Logout and Cleanup:

After finishing your work, log out from the target instance and then from the bastion host.
Terminate or stop instances as needed to avoid ongoing costs.
