PROJECT:0-Creating_static_website_on_EC2

# Creating a Static Website on Amazon EC2
In this project, I created a static website and hosted it on a web server running on an AWS EC2 instance. I needed to create a new EC2 instance, setup a connection to the instance, and install a web server to host the website. There was also some security group settings I needed to add in order to create an ssh connection to the instance and allow web traffic to your server. Once everything was configured, I was able to use the public address of the EC2 instance to access the site from a web browser.

### Key Resources
- **EC2 Instance types**: 
  - Instance types comprise various combinations of CPU, memory, storage, and networking capacity. It allows you to choose the appropriate mix of resources that is best suited for your applications.
  - Instances families are collections of instances grouped designed to meet a specific goal. Families include: General Purpose, Compute Optimized, Memory Optimized, Accelerated Computing, Storage Optimized.
  - T2.micro is the primary instance type offered in the free tier for most regions. In regions where t2.micro is not available, t3.micro is offered as an alternative under the Free Tier.
- **VPC Network Resources**: 
  - **Subnets**: A range of IP address in your VPC; must reside in a single availability zone. Public subnets have a direct route to an internet gateway while private subnets do not.
  - **Route Tables**: Determines where network traffic from your subnet or gateway is directed. When you create a VPC, a main route table is created for the VPC. You can create additional route tables for more granular control over the traffic on your network allowing you to create complex network architechtures.
  - **Internet Gateways**: Gateways connect your VPC to another network. An internet gateway provides a connection for your VPC to the internet. In addition to the internet, you can use gateways to connect to other networks including other VPCs or your on-premises network.
- **VPC security groups**:
  - Security groups act as a virtual firewall controlling what traffic can reach your instances.
  - Security group controls traffic that is allowed to reach the resources that it is associated with.
  - When you create a VPC, it comes with a default security group. You can also assign multiple security groups to a single resource. 
  - You can only specify allow rules, not deny rules.
  - When you first create a security group, it has no inbound rules meaning no inbound traffic is allowed until you add inbound rules to the security group. It has an outbound rule that allows all outbound traffic from the resource.
  - In order to add, update, or delete a rule, make sure you firstly have the required permissions to make changes.
  - When creating a rule, you can specify its protocol, description, source, and port range as needed.
- **SSH key pairs**:
  - A key pair consists of a public key and a private key used as a security tool for confirming your credentials when connecting to your ec2 instance. Amazon EC2 stores your public key on your instance, and you store the private key on your machine.
  - When you launch an instance, you can specify a key so you can connect to the intance. You can specify the same key pair for your instances or you can specify different key pairs.
  - For Linux instances, the private key allows you to securely connect to the instance. The private key is placed on your instance within ~/.ssh/authorized_keys, you must specify a private key that corresponds to a public key included in the file. For Windows intances, the private key is required to decrypt the administrator password.

### Services
This project will allowed me to gain experience in using several AWS services including:
- **Elastic Cloud Compute (EC2)**: Enables compute in the cloud. It provides you complete control of the computing resources and lets you run on Amazon's proven computing environment.
- **Virtual Private Cloud (VPC)**: Lets you provision a section of the AWS cloud where you can launch resources on a virtual network that you define. This includes being able to select your own ip address ranges, create subnets, and configure your routing tables and network gateways.
- **Identity and Access Management (IAM)**: Service for managing user access to AWS applications and services. It provides fine-grained access control through IAM policies. it is offered at no addtional charge.
#### Visualization
![image](./rolsamps/aws_projects/0-Creating_static_website_on_EC2/project-0.png)


### Capabilities
Working on this project provided an opportunity for me to develop skills in performing engineering tasks for the cloud including:
- **EC2**: Able to create ec2 instances, understanding different instance types, starting and stopping instance, use ssh to connect to instance, create new ssh key pairs.
- **Networking**: Able to create new VPC networks and subnets, create security groups, apply rules to allow ssh and http traffic.
- **IAM**: Can setup user accounts and provide access through policies, create new policies, applying principle of least privilege.
- **Linux OS**: Can install applications using the package manager, install and run a web server and host a website.

### Objectives
####  1. Launch a linux based EC2 instance in one region in a public subnet
####  2. Setup a security group that allows inbound https traffic from the internet and inbound ssh traffic from your ip address
####  3. SSH into the EC2 instance
####  4. Setup a web server on the EC2 instance
####  5. Add a hello world header on the home page of the website
####  6. Access the website from your browser using the public ip address of the EC2 instance

### Questions
####  What are Regions and Availability Zones?
- **Regions**: Different geographical areas, they are designed to be separate from other regions to acheive the greatest possible fault tolerance and stability. when choosing a region, select the region closest to the target customers or meets the legal or other requirements.
- **Availability zones**: Are multiple, isolated areas within a region, commonly a physically separated data center. Each AZ has its own power, cooling, networking, and physical security. AZs are represented by a region code followed by a letter identifier.
####  How are subnets determined for route tables?
- Each subnet in your VPC must be associated with a route table. A subnet can be explicitly associated with a custom route table, or implicitly or explicitly associated with the main route table.
####  What is the Principle of least privilege in IAM?
- The concept of least privilege is based on granting users with only the permission required to perform their tasks and nothing more. You can use IAM policies to define actions on specific resources and connect those resources to your account by assigning the policies.
- AWS managed IAM policies are pre-defined polcies availble for all customers to use which can help with getting started, but it is recommended to fine turn the permissions for your accounts by removing uneeded permissions. You can define your own custom policies and/or use IAM access analyzer to generate least-privilege polcies based on user activity logged in AWS Cloudtrail.
####  What is the Deny By Default Rule in Security Groups?
- Deny by default is a fundamental security principle that refers to their default behavior for Security groups of blocking all traffic unless explicitly allowed. It is recommended as best security practice to only create allow rules necessary for application functionality.
####  What are AMIs?
- An AMI (Amazon Machine Image) is a pre-configured template used to create EC2 instance. It contains the operating system, application server, and applications needed to launch an instance. When you launch an instance, the AMI that you choose must be compatible with the instance type that you choose.
####  What is a role identity and how does it compare to a user identity in IAM?
- **IAM role**: An identity that has specific permissions similar to an IAM user, but it is not associated with a specific person.
- **Temporary permissions**: Can be assumed temporarily to take on different permissions and perform a specific task.
- **Cross-account access**: Can provide cross account access to allow a trusted someone in a different account to access resources in your account.
- **Service role**:  An IAM role that a service assumes to perform actions in your behalf.
- **Application perimissions**: Can be used manage temporary credentials for applications that are running on an EC2 instance and making AWS CLI or AWS API requests.
#### What package manager can you use for Amazon linux?
- The default software package management tool in AL2023 is DNF. DNF is the successor to YUM, the package management tool in AL2.

### References
- **AWS EC2 Project reference from Shubham Murti**: https://github.com/shubhammurti/AWS-Projects-Portfolio/blob/main/Level%20100/3.%20Launch%20a%20Hello%20World%20website%20on%20the%20internet/COM03-AWS100%20-%20Launch%20a%20Hello%20World%20website%20on%20the%20internet.md
- **Amazon EC2 FAQs**: https://aws.amazon.com/ec2/faqs/
- **Amazon VPC FAQs**: https://aws.amazon.com/vpc/faqs/
- **Amazon EC2 instance types**: https://aws.amazon.com/ec2/instance-types/
- **Amazon Machine Images in Amazon EC2**: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html
- **Package management tool**: https://docs.aws.amazon.com/linux/al2023/ug/package-management.html
- **What is Amazon VPC?**: https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html
- **Authenticating with identities**: https://docs.aws.amazon.com/wickr/latest/adminguide-classic/security_iam_authentication.html
- **Create key pairs**: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/create-key-pairs.html
- **Connect to your EC2 instance**: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect.html
- **Apache vs Nginx: Practical Considerations**: https://www.digitalocean.com/community/tutorials/apache-vs-nginx-practical-considerations
- **Nginx beginners guide**: https://nginx.org/en/docs/beginners_guide.html

### Other Options
#### Connecting to your instance
  - **Connecting from Windows**: Connecting to a Linux EC2 instance from Windows is not possible natively, you will need an ssh client such as Putty to connect. [Download and install Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), and you will need to follow these steps to [ssh into ec2 from windows](https://stackoverflow.com/questions/5264945/ssh-to-ec2-linux-instance-from-windows)
  - **Amazon EC2 Instance Connect**: An AWS service that provides a secure way to connect to your Linux instance over SSH. You will use IAM policies and principals to control SSH access to your instances, removing the need to share and manage SSH keys. You can use Amazon EC2 Instance Connect to connect to your instances using the AWS console or a SSH client of your choice.
  - **AWS Session manager**: A system manager tool and a fully managed service that allows you to manage you amazon EC2 instances, edge devices, on-premise server, and virtual machines (VMs). You can use either an interactive one-click browser-based shell or the AWS Command Line Interface (AWS CLI).=
#### Apache vs Nginx
  - **Apache**: The most widely used web server application in the world. It benefits from great documentation and integrated support from other software projects. It is extensible through a dynamically loadable module system and can directly serve many scripting languages without requiring additional software. It is often chosen for its flexibility, power, and near-universal support.
  - **Nginx**: Created as an answer to the C10K problem, which was an outstanding challenge for web servers to be able to handle ten thousand concurrent connections and met this goal by relying on an asynchronous, events-driven architecture. It has become popular due to its lightweight footprint and its ability to scale easily on minimal hardware. It excels at serving static content quickly, has its own robust module system, and can proxy dynamic requests off to other software as needed. It is often chosen for its resource efficiency and responsiveness under load, as well as its straightforward configuration syntax.

### Tips
- **Security Best Practices when managing access SSH access to your EC2 instances**:
  - Never share your private keys.
  - Use strong passphrases for your key pairs.
  - Regularly rotate your keys.
  - Monitor and audit SSH access to your instances.
  - Consider using AWS Systems Manager Session Manager as an alternative to SSH for enhanced security.
  - Use security groups to restrict SSH access to trusted IP addresses only.
  - Consider implementing a bastion host for SSH access in production environments.
- **Linux Commands for running a nginx web server**:
  - **Systemctl commands**:
    - sudo systemctl status nginx
    - sudo systemctl stop nginx
    - sudo systemctl start nginx
    - sudo systemctl reload nginx
    - sudo systemctl restart nginx - force restart
  - **Nginx commands (Nginx has a set of built-in tools)**:
    - sudo /etc/init.d/nginx start
    - sudo /etc/init.d/nginx restart
    - sudo /etc/init.d/nginx stop
    - sudo /etc/init.d/nginx stop
    - sudo /etc/init.d/nginx reload
    - sudo nginx -s reload
    - sudo nginx -s quit




