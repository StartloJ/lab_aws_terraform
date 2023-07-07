# Create AWS EC2 with Terraform

This guideline will provide a step to provision EC2 instance by Terraform

## Initial

1. Create folder in `${__GIT_WORKING_DIR__}/components` with folder named `instances`.
2. Create files for `main.tf`, `versions.tf`, `providers.tf`, `variables.tf`, and `outputs.tf`.

## Step

1. Get an information to insert into EC2 creation module.
   1. VPC ID; you can see on web console or CLI.

        ```bash
        aws ec2 describe-vpcs
        ```

   2. Subnet ID; you can see on web console or CLI.

        ```bash
        aws ec2 describe-subnets --query 'Subnets[?VpcId==`vpc-0bc1a310dbceee75a`]'
        ```

   3. Region name; you should select a nearly geolocation.
   4. EC2 key pair

        ```bash
        aws ec2 describe-key-pairs
        ```

   5. AMI ID; you can see on web console

2. You should define a resource tag attributes like below

    ```terraform
    tags = {
       Managed_with = "terraform"
       Environment  = "opsta-workshop"
       Creator      = "${your_name}@opsta.co.th"
    }
    ```

3. Let's develop the Terraform code with AWS EC2 module. Read more [here](https://registry.terraform.io/modules/terraform-aws-modules/ec2-instance/aws/latest)

    ```terraform
    module "web_instance" {
      source = "terraform-aws-modules/ec2-instance/aws"
      ...
    }
    ```

4. Create a new Security group with Terraform. Read more [here](https://registry.terraform.io/modules/terraform-aws-modules/security-group/aws/latest)

    ```terraform
    module "web_app_sg" {
      source = "terraform-aws-modules/security-group/aws"
      ...
    }
    ```

5. Get an AMI image for provision EC2 compute

    ```terraform
    data "aws_ami" "amazonlinux" {
      executable_users = ["all"]
      most_recent      = true
      name_regex       = "^al2023-.*"
      owners           = ["amazon", "aws-marketplace"]
      filter {
        name   = "description"
        values = ["Amazon Linux 2023*"]
      }
      filter {
        name   = "root-device-type"
        values = ["ebs"]
      }
      filter {
        name   = "virtualization-type"
        values = ["hvm"]
      }
    }
    ```

6. Let's go work.
