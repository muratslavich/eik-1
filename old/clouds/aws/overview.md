# Overview

Amazon EC2 - elastic compute cloud

EBS - Elastic block store - storage volumes

AMI - Amazon machine images - provides the information required to launch an instance. Images.

IAM - Identity and Access management - access keys

VPC

### Region

Isolated from each other resources. You can see only resources from the specified region. Resources don't replicate automatically across different regions. There is a charge for data transfer between regions. Example:

* us-west-1;
* us-east-1;

[For more information](https://aws.amazon.com/about-aws/global-infrastructure/) about available regions.

### Zone

Instance run in Availability Zone.

Each Region has multiple, isolated locations known as _Availability Zones_. When you launch an instance, you can select an Availability Zone or let us choose one for you. If you distribute your instances across multiple Availability Zones and one instance fails, you can design your application so that an instance in another Availability Zone can handle requests.

[Regions and Zones.](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)

### Regional endpoint

You must specify an [endpoint](https://docs.aws.amazon.com/general/latest/gr/ec2-service.html) for the chosen region.

Example:

* ec2.us-east-1.amazonaws.com

### Create IAM user

Do not use AWS account root user.

* [Create administrator](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started\_create-admin-group.html)
* [Create a secret access key](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys)

The only time that you can view or download the secret access key is when you create the keys.

### Security group

Like a firewall for associated instances, it's controlling inbound and outbound traffic at the instance level.

You must add rules to a security group that enables you to connect to your instance using SSH.

Inbound rules:

* Choose HTTP from the Type list, and make sure that Source is set to Anywhere (0.0.0.0/0).
* Choose HTTPS from the Type list, and make sure that Source is set to Anywhere (0.0.0.0/0).
* Choose SSH from the Type list. In the Source box, choose My IP to automatically populate the field with the public IPv4 address of your local computer.

### Security key

Instance secure by the key pair and security group, when you connect you must specify the private key.

\--

### Prepare ec2

[set-up](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html)

* create IAM
* create a key pair
* create a security group

[launch](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2\_GetStarted.html#ec2-launch-instance)

* click launch instance
* chose AMI -  Ubuntu
* chose instance type - t2.micro
* configure details - stay default
* add storage - stay default
* add security group
* chose your private key

###
