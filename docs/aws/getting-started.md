# Unlocking the Cloud: A Guide to Getting Started on AWS - The Easy Way

In today's digital world, cloud computing isn't just a buzzword; it's an essential skill for both individuals and businesses aiming for growth, scalability, and cost-efficiency. Amazon Web Services (AWS), as one of the leading cloud platforms, offers a comprehensive suite of tools and services that cater to diverse computing needs. However, diving into AWS can seem overwhelming for those who are new to the cloud ecosystem. If that sounds like you, worry no more! This article is designed to be your roadmap for navigating AWS effortlessly.

Whether you're an entrepreneur looking for a cost-effective way to run your startup's website, a developer wanting to deploy applications at scale, or even a student aiming to get ahead with hands-on experience, this guide will walk you through the essentials of AWS. From setting up an AWS account to deploying your first application, we’ll cover it all in an easy-to-understand manner. By the end of this article, you'll be equipped with the knowledge and confidence to harness the power of AWS for your own projects.

So let’s clear away the clouds of confusion and step into the illuminating world of Amazon Web Services!

## Setting Up Your AWS Account

The first and most crucial step in embarking on your AWS journey is to set up an AWS account. Think of this account as your personal dashboard from which you can access the myriad of services offered by AWS. Here, you can track your usage, manage your services, and set your preferences. Setting up an AWS account is simple, straightforward, and takes only a few minutes. Let's walk through the process.

**Step 1: Navigate to the AWS Homepage**

- Open your web browser and go to the [AWS homepage](https://aws.amazon.com/).
- Click on the ^^Create an AWS Account^^ button.

**Step 2: Provide Basic Information**

- You'll be asked to enter your email address and an AWS account name.
- A verification email will be sent to the provided email address with a verification code.
- After the verification, you'll be asked to enter a password for your account. Make sure to choose a strong, unique password to ensure your account's security.

**Step 3: Enter Your Contact Information**

- Fill in your full name, phone number, and address. You can also choose whether this account will be for personal or business use.

**Step 4: Verify Your Identity**

- Amazon will send you a verification code via SMS or a phone call.
- Enter the code to verify your identity.

**Step 5: Payment Information**

- Even though many AWS services offer free tiers, you'll need to enter your credit card information for verification.
- Rest assured, you won't be charged unless you exceed the free tier limits or subscribe to paid services.

**Step 6: Confirm and Activate**

- After entering your payment details, you'll be taken to a confirmation page.
- Click on "Activate" to finalize the setup.
- Once your account is active, you'll be redirected to the AWS Management Console.

:tada: Congratulations! You've successfully set up your AWS account. Now you're ready to explore the diverse services that AWS has to offer. Remember, many of these come with a free tier option, so you can start experimenting without incurring extra costs.

## Safeguarding Your Root Account

Now that you've set up your AWS account, it's crucial to discuss the importance of securing your root account. The root account is essentially the **master key** to your AWS environment, giving you unrestricted access to all AWS services and resources. While this level of access is powerful, it also poses significant security risks if not managed correctly. Therefore, it's imperative to take steps to secure your root account right from the get-go. Here’s how you can do it:

**Step 1: Enable Multi-Factor Authentication (MFA)**

- Multi-Factor Authentication adds an extra layer of security by requiring two or more verification methods – a password and a security token.
- Navigate to the **IAM** (Identity and Access Management) section from your AWS Management Console.
- Under the dashboard, find **Activate MFA on your root account** and follow the steps to enable it.

**Step 2: Use a Strong, Unique Password**

- If you haven't already, make sure that your root account password is strong and unique.
- Consider using a reputable password manager to store and manage your passwords securely.

**Step 3: Limit Root Account Usage**

- The root account should only be used for tasks that absolutely require full administrative permissions.

!!! warning ""
    For regular tasks, create IAM users with the necessary permissions to limit the chances of unauthorized or accidental changes.

**Step 4: Regularly Review Account Activity**

- AWS provides detailed logging features like CloudTrail, which can be configured to monitor and log all activity in your AWS environment.
- Regularly review these logs to ensure that only authorized activities are being carried out.

!!! tip
    You can also set up CloudWatch alarms on CloudTrail specific events.

**Step 5: Set Up Account Recovery Options**

- Make sure you have set up and verified a recovery email and phone number.
- These will be critical for account recovery in case you lose access to your root account.

**Step 6: Keep Software and Systems Updated**

- Ensure that all devices used to access your AWS root account have the latest security patches and anti-malware software.
- This provides an additional line of defense against potential security threats.

**Bonus: Use AWS Organizations for Multiple Accounts**

- If you require multiple AWS accounts for various projects or departments, consider setting up **AWS Organizations**.
- This allows you to centrally manage billing, create and enforce policies, and apply security measures across all accounts.

By taking these security measures, you significantly reduce the risk of unauthorized access to your AWS resources. Remember, cybersecurity is an ongoing process, not a one-time setup. Always stay vigilant and keep abreast of best practices to ensure your AWS environment remains secure.

## Prepare Your AWS Account for Terraform

Terraform is an Infrastructure as Code (IaC) tool that allows you to build, change, and manage infrastructure in a straightforward, efficient manner. Using Terraform with AWS can make your cloud journey incredibly smooth, allowing you to automate resource deployment and scale effortlessly. Before diving into Terraform commands and script files, you'll need to prepare your AWS account for its integration. Here's how:

**Step 1: Install Terraform on Your Local Machine**

- You'll first need to download and install Terraform on your computer.
- Follow the [official installation guide](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) based on your operating system.

**Step 2: Create an IAM User for Terraform**

- Log into your AWS Management Console and navigate to the IAM (Identity and Access Management) section.
- Create a new IAM user with programmatic access. This will generate an Access Key ID and Secret Access Key.
- Assign the necessary permissions to this IAM user. It’s good practice to follow the principle of least privilege, only granting the permissions required for your specific use-case.

**Step 3: Configure AWS CLI with the IAM User Credentials**

- If you haven’t already, [install the AWS CLI](https://aws.amazon.com/cli/) on your computer.
- Run `aws configure` in the terminal and input the Access Key ID and Secret Access Key for the IAM user you created.

**Bonus: Create an S3 Bucket and a DynamoDB Table for Terraform**

- Log into your AWS Management Console and navigate to the S3 (Simple Storage Service) section.
- Create a new bucket. It will be used to remotely store Terraform state files.
- Navigate to the DynamoDB section.
- Create a DynamoDB Standard-IA table with on-demand capacity. It will be used to manage Terraform state lock to avoid parallel execution of the same Terraform project.

**Bonus#2: Create Needed AWS Resources by Using a CloudFormation Template**

- Log into your AWS Management Console and navigate to the CloudFormation section.
- Create a stack by using the CloudFormation template bellow.
- When the stack is completely deployed, you still need to configure AWS CLI with the IAM User credentials. You can retrieve the credentials on Secrets Manager section.

This CloudFormation stack create the following resources:

- An IAM User **terraform** with **AdministratorAccess** policy and an Access Key for programmatic access.
- A secret **iam/user/terraform/credentials** on Secrets Manager to store Access Key sensitive values.
- An S3 bucket **terraform-state-${AWS::AccountId}** with versioning enabled and a bucket policy that ^^only allow terraform user to perform GET, PUT and DELETE actions on objects^^.
- A DynamoDB table **terraform-state-lock**.

!!! note
    This CloudFormation template can be improve by setting encryption using custom KMS keys, restrict rights of the Terraform user and restrict access of the Terraform resources.

```yaml title="aws-cfn-terraform-init.yaml" linenums="1"
AWSTemplateFormatVersion: 2010-09-09

Resources:
  TerraformUser:
    Type: AWS::IAM::User
    Properties:
      UserName: terraform
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/AdministratorAccess

  TerraformUserAccessKey:
    Type: AWS::IAM::AccessKey
    Properties:
      UserName: !Ref TerraformUser

  TerraformUserSecret:
    Type: AWS::SecretsManager::Secret
    Properties:
      Name: !Sub iam/user/${TerraformUser}/credentials
      Description: Access Key of terraform user
      SecretString: !Sub '{"AWS_ACCESS_KEY_ID":"${TerraformUserAccessKey}", "AWS_SECRET_ACCESS_KEY":"${TerraformUserAccessKey.SecretAccessKey}"}'

  TerraformBucket:
    Type: AWS::S3::Bucket
    DeletionPolicy: Retain
    Properties:
      BucketName: !Sub terraform-state-${AWS::AccountId}
      VersioningConfiguration:
        Status: Enabled

  TerraformBucketPolicy:
    Type: AWS::S3::BucketPolicy
    Properties:
      Bucket: !Ref TerraformBucket
      PolicyDocument:
        Statement:
          - Sid: DenyDeleteObject
            Effect: Deny
            Principal: "*"
            Action: "s3:DeleteObject"
            Resource:
                - !Sub ${TerraformBucket.Arn}/*
            Condition:
              ArnNotEquals:
                aws:PrincipalArn: !GetAtt TerraformUser.Arn

  TerraformDynamoDBTable:
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: !Sub terraform-state-lock
      BillingMode: PAY_PER_REQUEST
      TableClass: STANDARD_INFREQUENT_ACCESS
      AttributeDefinitions:
        - AttributeName: LockID
          AttributeType: S
      KeySchema:
        - AttributeName: LockID
          KeyType: HASH
```

## Deploying a VPC and Subnets with Terraform

!!! warning
    This part may incur costs on your AWS account.

After preparing your AWS account to work with Terraform, it's time to delve into more advanced functionalities. One of the foundational aspects of any cloud deployment is the network, and in AWS, a Virtual Private Cloud (VPC) often serves as the backbone of your networking infrastructure. Using Terraform, you can automate the deployment of a VPC along with its associated subnets. Let's dive into how you can do this:

**Step 1: Initialize Your Terraform Project Structure**

- Create a new folder for the Terraform project. You can reproduce the structure bellow.

```
tf-project/
├─ backend.tf
├─ main.tf
└─ providers.tf
```

- The `backend.tf` file will contains the configuration of our S3 remote backend.
- The `main.tf` file will contains the configuration of our VPC.
- The `providers.tf` file will contains the configuration of the AWS provider.

**Step 2: Configure the S3 Remote Backend**

- Add the S3 remote backend configuration on the `backend.tf` file.

```hcl title="backend.tf" linenums="1"
terraform {
    backend "s3" {
        bucket         = "terraform-state-<account-id>" # (1)!
        dynamodb_table = "terraform-state-lock"
        encrypt        = true
        key            = "vpc/terraform.tfstate"
        region         = "eu-west-1"
    }
}
```

1. :writing_hand: Don't forget to replace `<account-id>` by your account ID.

As you can see, we are using the ^^S3 bucket^^ and the ^^DynamoDB table^^ provisioned earlier to manage our Terraform backend.

The `key` attribute will be the object name on the S3 bucket.

**Step 3: Configure the AWS Provider**

- Add the AWS provider configuration on the `providers.tf` file.

```hcl title="providers.tf" linenums="1"
provider "aws" {}
```

The AWS provider configuration is get from the AWS CLI configuration so we don't need to overide it.

**Step 4: Define the VPC Configuration**

- Define the VPC configuration on the `main.tf` file.

```hcl title="main.tf" linenums="1"
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.1.2"

  name = "my-vpc"
  cidr = "10.0.0.0/16"

  azs = ["eu-west-1a", "eu-west-1b"]

  database_subnets    = ["10.0.0.0/24", "10.0.1.0/24"]
  private_subnets     = ["10.0.10.0/24", "10.0.11.0/24"]
  public_subnets      = ["10.0.100.0/24", "10.0.101.0/24"]

  enable_nat_gateway = true
}
```

To simplify the VPC configuration we will use a Terraform module developed by AWS. This module allow us to configure many ressources with few informations.

Here we will deploy a VPC `my-vpc` with `10.0.0.0/16` as base CIDR block. On this VPC we will create 3 groups of 2 subnets with specific purpose.

- **Database subnets** will be used by database resources. The resources on these subnets cannot be accessed directly from internet.
- **Private subnets** will be used by any resource that is not a database or publicly accessible. The resources on these subnets cannot be accessed directly from internet.
- **Public subnets** will be used by any resource that can be publicly accessible. The resources on these subnets can be accessed from internet.

!!! note
    A public subnet is defined by its route table.
    If the default route of the attached route table redirect the traffic to an Internet Gateway the subnet is considered as public.

    A resource on a public subnet is not publicly accessible by default. You have to configure a security group that enable public access.

**Step 5: Initialize Your Terraform Project**

- Open a terminal and navigate to the Terraform project
- Run `terraform init` to initialize the Terraform project and download necessary providers and modules.

**Step 6: Validate the Configuration**

- Run `terraform validate` to validate the configuration.

**Step 7: Review and Deploy**

- Run `terraform plan` to review the actions that Terraform will take.
- If the plan looks as expected, apply the configuration by executing `terraform apply`. You will be asked to type `yes` to confirm the deployment.

!!! tip
    It's recommended to use `terraform plan -out=FILENAME` and `terraform apply FILENAME` to only make actions that have been displayed on the plan.

**Step 8: Confirm the Deployment**

Once the process is complete, you should see an output displaying the IDs and other details of the resources you've just created.

:tada: Congratulations, you've successfully automated the deployment of a VPC and subnets in AWS using Terraform! These foundational components will serve as the backbone for your cloud applications and services, allowing for greater scalability and flexibility.

## Deploying an EC2 Instance in a New Terraform Project Using Outputs from the VPC Project

In many real-world scenarios, infrastructure is managed across multiple Terraform projects for better modularity, reusability, and separation of concerns.

In the previous section we have deployed a VPC using a dedicated Terraform project and we want to use those resources in another project to deploy an EC2 instance. This guide walks you through how to do just that, leveraging the output variables from the VPC project for use in your EC2 project.

**Step 1: Generate Outputs for the VPC Project**

- Add `outputs.tf` new file on the VPC Terraform project.
- Define the outputs for the VPC project.

```hcl title="outputs.tf" linenums="1"
output "private_subnet_ids" {
  value = module.vpc.private_subnets
}
```

Here we will add an output `private_subnet_ids` that consist of a list of the private subnet IDs.

- Run `terraform apply` to add this output to the Terraform state.

**Step 2: Initialize Your EC2 Terraform Project**

- Create a new folder for the EC2 Terraform Project.
- To simplify the configuration we will define our configuration on a single file. Create `main.tf` new file.

```hcl title="main.tf" linenums="1"
terraform {
    backend "s3" {
        bucket         = "terraform-state-<account-id>"
        dynamodb_table = "terraform-state-lock"
        encrypt        = true
        key            = "ec2/terraform.tfstate" # (1)!
        region         = "eu-west-1"
    }
}

provider "aws" {}

data "terraform_remote_state" "vpc" {
  backend = "s3"
  config = {
    bucket         = "terraform-state-<account-id>"
    dynamodb_table = "terraform-state-lock"
    encrypt        = true
    key            = "vpc/terraform.tfstate"
    region         = "eu-west-1"
  }
}

module "ec2-instance" {
  source  = "terraform-aws-modules/ec2-instance/aws"
  version = "5.5.0"

  name = "my-ec2"

  instance_type = "t2.micro"
  subnet_id = data.terraform_remote_state.vpc.outputs.private_subnet_ids[0] # (2)!
}
```

1. :warning: Don't forget to change the backend key.
2. We use the first private subnet id.

To get data from another Terraform project we can use `terraform_remote_state` to retrieve data from the remote state of the targeted project.

**Step 3: Initialize, Validate, Review and Deploy**

- Run `terraform init` to initialize the Terraform project.
- Run `terraform validate` to validate the configuration.
- Run `terraform plan` to review the actions that Terraform will take.
- Run `terraform apply` to apply the configuration.

:tada: Congratulations! You've now successfully deployed an EC2 instance in a new Terraform project, reusing the output variables generated from your initial VPC project. This modularity is one of Terraform's strong suits, enabling scalable and maintainable infrastructure as code practices.

## Cleaning Up AWS Resources

While Terraform is fantastic for provisioning resources, it’s equally competent at tearing them down when they're no longer needed. It’s a critical aspect of Infrastructure as Code, especially when you want to manage costs or decommission a project. This guide walks you through the steps required to safely destroy the Terraform projects we have deployed in AWS.

**Step 1: Plan the Destruction of EC2 Project**

We need to destroy the EC2 project first because it depends on resources deployed by the VPC project.

- With a terminal, navigate to the EC2 project.
- Run `terraform plan -destroy` to review the actions that Terraform will take to destroy the project.

**Step 2: Execute the Destruction of EC2 Project**

- Run `terraform destroy` when you are certain about what you're destroying. You will be asked to type `yes` and hit enter to validate the operation.

**Step 3: Do the same for VPC Project**

- With a terminal, navigate to the VPC project.
- Run `terraform plan -destroy` to review the actions that Terraform will take to destroy the project.
- Run `terraform destroy` when you are certain about what you're destroying. You will be asked to type `yes` and hit enter to validate the operation.

After the command completes, log into your AWS Console and navigate to the appropriate sections (EC2 Dashboard, VPC Dashboard, etc.) to confirm that the resources have been terminated or deleted.

And just like that, you’ve safely decommissioned your Terraform project and cleaned up your AWS environment. While the terraform destroy command is powerful, it should be used judiciously. Always make sure to double-check the resources that will be affected and back up essential data before proceeding.
