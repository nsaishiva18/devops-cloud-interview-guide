# Q3. What is the role of statefile in Terraform?

## Answer
The Terraform statefile is a critical component(Heart of Terraform) that keeps track of the infrastructure resources managed by Terraform. It acts as a source of truth for the current state of the infrastructure, allowing Terraform to determine what changes need to be made during apply operations.

- Terraform creates a resource on cloud provider like AWS, Azure, or GCP. Terraform stores this information in a statefile to understand what resources it is managing.

- So in future, if it has to update the resource that it created, it will refer to the statefile to know the current configuration of the resource.

- EC2 Instance --> Resource ID, Metadata, IP address, last known state.

- Whenever you run `terraform plan` or `terraform apply`, Terraform gives a prompt saying that these are the additional resources will be modified or deleted.

## Q4. Have you considered storing statefile in Git instead of AWS S3 or Azure Blob ?

## Answer
In Terraform, statefile holds the important information, such as resource IDs, metadata, dependencies, and last known state of the infrastructure. As this is sensitive info, it should not be stored in local disk or Git repository. There is a risk of exposing sensitive data.

- So to avoid this state, file is stored in remote backends like AWS S3, Azure Blob Storage, Google Cloud Storage, 

- We need to define backend.tf file in remote backend such as S3 on AWS. Provide name, region and other information so that terraform can upload the state file on remote backends.


# Q5. Explain Terraform Statefile Management?

## Answer

`Statefile` - heart of Terraform. Stores sensitive info like resources, dependencies, metadata, last known state, etc.

- `Statefile Management` - is a process of deciding where to store the statefile and how to manage it.

- It has to be in centralized location.
- Terraform supports remote backends for statefile management like AWS S3, Azure Blob Storage, Google Cloud Storage.

- Benefits of remote backends:
  - **Collaboration**: Multiple team members can work together without conflicts.
  - **Versioning**: Track changes to the statefile over time.
  - **Security**: Control access to sensitive information.
  - **Locking**: Prevent concurrent modifications to the statefile.
  - **Backup**: Automatic backups for disaster recovery.
  - **RBAC**: Role-Based Access Control to manage permissions effectively.


# Q6. Two DevOps Engineers attempts to update statefile at once. What happens?

## Answer

It can lead to conflicts and inconsistencies in the state of the infrastructure. This situation is known as a "statefile conflict."

- Remote backend has a locking mechanism to prevent concurrent modifications to the statefile.
- The first engineer will acquire a lock on the statefile when they start their operation.
- The second engineer will be blocked from making changes until the first engineer releases the lock.

# Q7. We don't have a cloud account Where can we store the statefile ?

## Answer
Remote backend concept of terraform is not restricted to cloud providers. We can check HasdhiCorp page for supported remote backends.
- Terraform supports various remote backends for statefile management, including:
  - **HashiCorp Consul**: A distributed, highly available system for service discovery and configuration.
  - **Terraform Cloud**: A SaaS platform by HashiCorp that provides remote state management and collaboration features.
  - **Local backend with locking**: Using local files with a locking mechanism to prevent concurrent access.
  - **Other third-party backends**: Some organizations may use custom or third-party solutions for statefile management.

# Q8. Do you use Terraform Enterprise or Community version ?

## Answer
I use the Community version of Terraform for most of my projects. It provides all the essential features needed for infrastructure as code, including support for various providers, modules, and remote backends.

- However, I am aware of Terraform Enterprise, which offers additional features such as enhanced security, governance, and collaboration tools. Depending on the project's requirements and team size, I would consider using Terraform Enterprise for larger organizations that need advanced capabilities like policy enforcement, audit logging, and role-based access control.

- Terraform is our single source of truth or single point of contact for creating the infrastructure. No developer can manually update or delete the infrastructure on cloud provider. Everything has to be done from Terraform. So there is no point of drift detection. 

- Enterprise version provides better statefile management with RBAC and drift detection.

- Remote backend
- Proper versioning
- locking mechanism
- Security 
- drift detection

# Q9. Have you heard about opentofu ? Do you think it is better than Terraform ?

## Answer

Opentofu and Terraform are both  are IAC tools and share their roots because open tofu was actually created form terraform codebase, because of liciensing change of terraform. 

- We are using terraform to create internal infructure and we don't sell terraform. So we are not affected by the licensing changes of terraform.



# Q10. Write terraform code to create any resource on AWS ?

## Answer

This is to test if you are aware of actual flow of terraform.
S3. Will start with main.tf, with in the main.tf will start with defining a terraform block.

- Within terraform block will define required provider as AWS and version of provider. --> AS it has to download the provider plugin from hashicorp registry.

# main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
  required_version = ">= 1.0.0"
}
provider "aws" {
  region = "us-west-2"
}

# s3.tf
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-unique-bucket-name-12345"
  acl    = "private"

  tags = {
    Name        = "My bucket"
    Environment = "Dev"
  }
}

**Note:-** If the configuration is small will write S3 resource in main.tf itself. If it is big infra, will create multiple .tf files for better organization.

- Instead of hardcoding region,the values of the fields, will creae Variables.tf file and will customize the values.


# Q11. What is the difference between Resource adn Datasource in Terraform ?

## Answer

Terraform can not only create, delete and modify the cloud resources, but also read the existing resources.
- **Resource**: A resource in Terraform represents a component of your infrastructure that you want to create, update, or delete. Resources are defined using the `resource` block in Terraform configuration files. For example, creating an EC2 instance, S3 bucket, or VPC are all examples of resources.

**example:**

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

- **Datasource**: A datasource in Terraform is used to fetch and read information about existing resources that are not managed by your Terraform configuration. Datasources are defined using the `data` block. They allow you to reference existing infrastructure components, such as retrieving details about an existing VPC or AMI, without creating or modifying them.   

**example:**
```hcl
data "aws_vpc" "default" {
  default = true
}

```

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = data.aws_vpc.default.default_route_table_id  # We will use data. to read the information                           
}
```

In a nutshell,

`Resources`- Used to create. update or delete infrastructure components.

`Datasource`- used to read and fetch information with in terraform without modifying them.

