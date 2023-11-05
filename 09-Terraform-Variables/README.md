# Terraform Variables


In Terraform, variables are placeholders that allow you to input data into your configurations, making your Terraform code more flexible and reusable.

- Variables are containers for **storing and managing values, such as text, numbers, or complex data**. These values can be used within your Terraform code.

- Variables are used to **parameterize your configurations**, allowing you to customize your infrastructure. 
- You can define variables for various settings like *regions, instance types,* etc.

- Variables are declared in your Terraform code using the ***`variable`*** block. You specify the **variable's name**, **type**, and an **optional default value**.

- When running Terraform, you can input values for variables, either by specifying them in a separate variable file or through command-line arguments.

- Variables make your code reusable. You can use the same Terraform configuration with different input values, making it adaptable to different environment (prd, pre, uat, dev).

- **Example**:  

    [00_provider.tf](./00_provider.tf)
    ```hcl
    terraform {
    required_providers {
        aws = {
        source  = "hashicorp/aws"
        version = "~> 5.0"
        }
    }
    }

    provider "aws" {
    #region = "us-east-1"
    region = var.aws_region

    default_tags {
        tags = {
        Terraform = "yes"
        #Owner = "Venkatesh"
        Owner = var.owner
        }
    }
    }
    ```

    [01_ec2.tf](./01_ec2.tf)
    ```hcl
    resource "aws_instance" "myec2" {
    # terraform arguments without variables
    # ami = "ami-0df435f331839b2d6"
    # instance_type = "t2.micro"
    # count = 1

    # using variables for arguments
    ami           = var.ec2_ami
    instance_type = var.ec2_instance_type
    count         = var.instance_count

    tags = {
        Name = "Linux2023"
    }
    }
    ```

    [02_variables.tf](./02_variables.tf)
    ```hcl
    variable "aws_region" {
    type    = string
    default = "us-east-1"
    }

    variable "owner" {
    type    = string
    default = "Venkatesh"
    }

    variable "ec2_ami" {
    type    = string
    default = "ami-0df435f331839b2d6" # Amazon Linux 2023
    }

    variable "ec2_instance_type" {
    type    = string
    default = "t2.micro"
    }

    variable "instance_count" {
    type    = number
    default = 1
    }
    ```


- In the above example, We've defined five variables: 
    1. `aws_region`: 
    2. `owner`: 
    3. `ec2_ami`: 
    4. `ec2_instance_type`:
    5. `instance_count`: 

- Each variable has a specific **data type** (*string* or  *number*) and *optional default values*.

- With these variables, you can customize the AMI, instance type, and the number of EC2 instances you want to create, making your Terraform configuration adaptable and flexible

- ***`terraform plan`*** Output

```hcl
$ terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:      
  + create

Terraform will perform the following actions:

  # aws_instance.myec2[0] will be created
  + resource "aws_instance" "myec2" {
      + ami                                  = "ami-0df435f331839b2d6"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_stop                     = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + host_resource_group_arn              = (known after apply)
      + iam_instance_profile                 = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_lifecycle                   = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = (known after apply)
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns                          = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns                           = (known after apply)
      + public_ip                            = (known after apply)
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + spot_instance_request_id             = (known after apply)
      + subnet_id                            = (known after apply)
      + tags                                 = {
          + "Name" = "Linux2023"
        }
      + tags_all                             = {
          + "Name"      = "Linux2023"
          + "Owner"     = "Venkatesh"
          + "Terraform" = "yes"
        }
      + tenancy                              = (known after apply)
      + user_data                            = (known after apply)
      + user_data_base64                     = (known after apply)
      + user_data_replace_on_change          = false
      + vpc_security_group_ids               = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```