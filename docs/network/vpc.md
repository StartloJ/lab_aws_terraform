# Create network with terraform

Let's start with me.

## Initial

1. Create folder in `${__GIT_WORKING_DIR__}/components` with folder named `network`.
2. Create files for `main.tf`, `versions.tf`, `providers.tf`, `variables.tf`, and `outputs.tf`.

## Init content

- version.tf

    ```tf
    terraform {
      required_version = ">= 1.0.0"

      required_providers {
        aws = {
          source  = "hashicorp/aws"
          version = ">= 3.72"
        }
      }
    }
    ```

- providers.tf

    ```tf
    provider "aws" {
      region = local.region
    }
    ```

## Create Network on AWS

Now, we can let's start to provision a network on AWS provider.

### Use VPC module, see more detials [here](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest)

1. Go to `main.tf` to insert content.

   - Add module `vpc` with name `this`
   - `source` is `terraform-aws-modules/vpc/aws`
   - `name` is your own creation.
   - `cidr` is 10.x.0.0/16
   - `azs` is select one or more of a, b, or c
   - `private_subnets` is under cidr and don't overlap `public_subnets`
   - `public_subnets` is under cidr and don't overlap `private_subnets`
   - `enable_nat_gateway` is **true**
   - `create_igw` is **true**
   - `tags` is below
      - Managed_with = "terraform"
      - Environment = "opsta-workshop"
      - Creator = "${YOUR_EMAIL}@opsta.co.th"
2. Go to `variables.tf` to insert dynamic values.

   - Create single VPC with a separate variables.
   - If we need to create multiple VPC, How can we?

### Use VPC endpoint module, see more detials [here](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest/submodules/vpc-endpoints)

1. Go to `main.tf` to insert content.

   - Add module `endpoints` with name `this`
   - `source` is `terraform-aws-modules/vpc/aws//modules/vpc-endpoints`
   - `vpc_id` is ref from previous module result.
   - `endpoints` for S3
