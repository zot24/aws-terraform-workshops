# Steps W1

* Login to your [AWS account console](https://console.aws.amazon.com/vpc/?#).
* Clone git repository with workshop data, go to w1 directory:

```ssh
$ git clone https://github.com/zot24/aws-terraform-workshops.git
$ cd aws-terraform-workshops/w1
```
* Configure Terraform with AWS credentials (see pre-requisites) in provider.tf configuration file:

```ssh
$ cat provider.tf
provider "aws" {
  access_key = "THISISEXAMPLETHISISEXAMPLE"
  secret_key = “THISISEXAMPLE/kToJ5qUtCpxr/THISISEXAMPLE"
  region = "us-east-1"
}
```

* Follow terraform documentation for EC2 instance and comments in main.tf to complete configuration:
  * Go to AWS VPC console, write down VPC ID and subnet ID which are required to complete configuration.
Please notice that VPC and subnets are covered in details in the next workshops but they are still required to finish EC2 configuration.
  * Use t2.nano as instance type for EC2 instance.
  * You should specify names for AWS resources as well as missing configuration parameters.
  * In this workshop we need to create EC2 instance in its own security group, see documentation [here](https://www.terraform.io/docs/providers/aws/r/security_group.html) and [here](https://www.terraform.io/docs/providers/aws/r/instance.html).
  * Run terraform plan to make sure configuration is ready to be applied.
  * Run terraform apply to actually create AWS resources: EC2 security group and EC2 instance.
* Go to AWS console and find newly created EC2 instance and security group.
* Open terraform.tfstate to examine its structure and newly created AWS resources. Please don't make any changes into this file. It's managed by terraform so manual changes into this file may break things up.
* Modify EC2 instance type:
  * Change EC2 instance type from t2.nano to t2.micro.
  * Run terraform plan and then terraform apply to actually apply changes.
  * Check changes in AWS EC2 web console.
* Add your SSH key to EC2 instance and access it via SSH.
  * Uncomment user_data parameter in terraform config.
  * Replace example SSH key with your public SSH key to shared/user-data.txt file:

```ssh
cd ../
$ cat shared/user-data.txt
#!/bin/bash
   
mkdir -p /home/ec2-user/.ssh
cat <<FILE > /home/ec2-user/.ssh/authorized_keys
ssh-rsa AAAAEXAMPLEyc2EAAAADAQABAAABAQCxz1G2vfqCabgFNZiL/timcrISitT4ShZP0iB4G1F+tFRM7to3CstEbS9TFeZwJdKeKLJoGsB5mMueqQb34lVt+ieodNKn8vMjTqv62W8YLqhRavnJ7bTGqGxNhAuLZJdEXAMPLEgywFwQjKYIVQt0SeB0XXLgAUIp+FS7MVyywDdViLqHWexxFN9Nrd6nPAj0fLV9DRIwe7nRccj+R4HvGIC7rQ060QCDCssYiZT/FVihNcPfohQA1JlNGao/lXLkSivwtl0pEDECyzs2KULS+9mc5Bz0Ap1Liskoa5V9umz8LhA9WLqNaCtt6fWQurPAd5lpEXAMPLE user@host
FILE
chown ec2-user.ec2-user /home/ec2-user/.ssh/authorized_keys
chmod 400 /home/ec2-user/.ssh/authorized_keys
yum -y erase docker
```

  * Apply configuration changes.
  * Login to newly created EC2 instance via SSH.
  * Run commands uptime, top, uname -a on EC2 instance.
* Run terraform destroy to delete AWS resources which were created during this workshop.
Hint: in case you got stuck during this workshop you can check answers directory in [aws-terraform-workshops](https://github.com/Smartling/aws-terraform-workshops.git) git repository.
