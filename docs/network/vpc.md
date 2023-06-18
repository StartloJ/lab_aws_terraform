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
      backend "s3" {
        bucket               = "tf-statefile-all"
        region               = "ap-southeast-1"
        workspace_key_prefix = "network"
        key                  = "terraform.tfstate"
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

- Go to `main.tf` to init content.
