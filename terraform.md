## Terraform
- Terraform is an open-source infrastructure as code (IaC) tool that allows you to define, provision, and manage infrastructure across multiple cloud providers using a declarative configuration language called HCL (HashiCorp Configuration Language).


Q. How does Terraform work?
- Terraform works by reading configuration files written in HCL (HashiCorp Configuration Language) that define the desired state of your infrastructure. It then compares the desired state with the current state of your infrastructure (as stored in the state file) and generates a plan of actions to bring your infrastructure into the desired state. Once you approve the plan, Terraform applies the changes by making API calls to the appropriate cloud providers.

---
Q. What is a Terraform workspace?
- A Terraform workspace allows you to manage multiple environments (such as development, staging, and production) using the same configuration files. Each workspace has its own state file, allowing you to manage different sets of resources for each environment.

---
Q1: Suppose you created an ec2 instance with terraform and after creation, you have removed the entry from the state file now, when you run terraform apply what will happen?

> As we have removed the entry from that state file so terraform will no longer manage that resource so on the next apply it will `create` a new resource.

---
Q2: What is a state file in Terraform?

> A state file is a file in which Terraform keeps track of all the infrastructure that is deployed by it.

---
Q3: What is the best way to store the terraform state file?

> The best way to store the state file is to keep it in the remote backend like S3 so, that whenever multiple people are working on the same code resource duplication won’t happen.

---
Q4: What is terraform state locking?

> Whenever we are working on any terraform code and do terraform plan, apply or destroy terraform will lock the state file in order to prevent the destructive action.

---
Q5: What is Terraform backend?

> A backend defines where Terraform stores its state data files.

---
Q6: What is a plugin in Terraform?

> The plugin is responsible for converting the HCL code into API calls and sends the request to the appropriate provider (AWS, GCP)

---
Q8: What are the types of provisioners?

Remote exec: Run commands using Terraform on a remote server
Local exec: Run commands using Terraform on the local system

---
Q9: What is the use of Terraform module?

- We can create the terraform modules one time and reuse them whenever needed
- To make to code standardized
- To reduce the code duplication
- The module can be versioned

---
Q10: If I have created EC2 and VPC using Terraform and unfortunately tfstate file got deleted, can you recover it? (File is only on the local machine not on s3 or dynamo DB)

> You can import the resources that are created by Terraform using `terraform import` command and then it will come to the state file

---
Q11: If we have created different-different modules like VPC, EC2, security group, access key, and subnet so how terraform will get an idea of which resource should deploy first?

- Terraform automatically figures out the dependency graph based on the resource references in your code. 
- It understands the relationships between resources, and it uses this information to determine the order in which the resources should be created or modified.
- You can define the explicit dependency with the `depends_on` keyword

---
Q12: How I can delete/destroy specific resources without changing logic?

- Using taint and destroy command
- We need to taint that resource using `terraform taint RESOURCE_TYPE.RESOURCE_NAME` command
- After tainting the resource, you can run the “destroy” command to remove the tainted resources using `terraform destroy -target=RESOURCE_TYPE.RESOURCE_NAME` command

---
Q13: How can we rename a resource in Terraform without deleting it?

We can rename a resource without deleting it using `terraform mv` command

---
Q14: Let’s say you have created an EC2 instance using Terraform and someone does the manual change on it next time you run Terraform plan what will happen?

- Terraform state will be mismatched and terraform will modify the EC2 instance to the desired state i.e. whatever we have defined in the .tf file

---
Q15: What is the difference between locals & variables in terraform?

- The variables are defined in the variables.tf file or using variables keyword that can be overridden but the locals can not be overridden.
- So if you want to restrict the overriding the variables at that time you need to use the locals.

---
Q. What is the purpose of the "Terraform refresh" command, and when would you use it?

- The "Terraform refresh" command retrieves the current state of the infrastructure resources and updates the Terraform state file to match the real-world resources. It is useful when changes have been made outside of Terraform's control and the state file needs to be updated to accurately reflect the actual state of the infrastructure.

---
Q. Explain the concept of remote state locking in Terraform and its importance in team collaboration.

- Remote state locking in Terraform prevents concurrent modifications to the same state file by multiple users. When a user runs a Terraform command that modifies the state, the lock is acquired to prevent conflicts.

- This ensures consistency and prevents data corruption in team-based workflows, where multiple users might be working on the same infrastructure.

---
Q. What is Terraform’s “target” argument and how can it be useful?

- The “target” argument in Terraform allows you to specify a single resource or module to be targeted for an operation. This feature is particularly useful when you want to apply changes only to specific resources without affecting the entire infrastructure. For example:

> terraform apply -target="aws_security_group.my_sg"

---
Q. What is the purpose of the terraform_remote_state data source and how is it used?
- The terraform_remote_state data source in Terraform enables sharing and retrieving outputs from a separate Terraform state file.

```sql
data "terraform_remote_state" "networking" {
 backend = "s3"
  config = {
     bucket = "example-bucket"
     key = "networking.tfstate"
     region = "us-west-2"
   }
}
resource "aws_instance" "example" {
 // Use the remote state output as input for resource configuration
 subnet_id = data.terraform_remote_state.networking.outputs.subnet_id
 // …
}
```
---
Q4. ) You have a Terraform configuration file that defines an infrastructure deployment. However, there are multiple instances of the same resource that need to be created. How would you modify the configuration file to achieve this?

- To create multiple instances of the same resource in Terraform, you can use the `count parameter or the for_each parameter`, depending on your Terraform version. These parameters allow you to specify how many instances of a resource you want to create and provide a way to iterate over a set of values to define unique instances.

### Using count:
```sql
provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "example" {
  count = 3  # Set the number of instances you want

  ami           = "ami-12345678"
  instance_type = "t2.micro"

  # other resource attributes...
}
```
### Using for_each (Terraform 0.12 and later):
```sql
provider "aws" {
  region = "us-west-2"
}

locals {
  instances = {
    "example1" = { ami = "ami-12345678", instance_type = "t2.micro" },
    "example2" = { ami = "ami-87654321", instance_type = "t2.micro" },
    "example3" = { ami = "ami-98765432", instance_type = "t2.micro" },
  }
}

resource "aws_instance" "example" {
  for_each = local.instances

  ami           = each.value.ami
  instance_type = each.value.instance_type

  # other resource attributes...
}
```
Choose between count and for_each based on your specific requirements. If you need to create a fixed number of instances, count is a simpler option. If you need to create instances based on a dynamic set of keys (e.g., instances with unique names), for_each provides more flexibility.

---
Q. Which module is used to store the .tfstate file in S3?

- The module used to store the .tfstate file in Amazon S3 is the "S3 backend" in Terraform. The S3 backend allows you to store your Terraform state files in an Amazon S3 bucket. This is often used for remote state storage, which provides benefits like concurrent state locking and collaboration among team members.

---
Q. How do you manage sensitive data in Terraform, such as API keys or passwords?

1/ Sensitive Variables: You can mark variables as sensitive to prevent their values from being displayed in the Terraform console output or saved in the Terraform state file. For example:
```sql
variable "api_key" {  
   type        = string   
   description = "API key"   
   sensitive   = true 
}
```

When you use this variable, its value won’t be displayed in the console or logged in the state file.

2. Sensitive Outputs: Similarly, you can mark an output as sensitive to prevent its value from being displayed in the console output or saved in logs:
```sql
output "secret_output" {
  value       = some_sensitive_value()
  sensitive   = true
}
```
3. Environment Variables: Storing sensitive information in environment variables and referencing them in your Terraform configuration can be a secure approach. Use the env function to access environment variables:
```sql
variable "api_key" {}

provider "aws" {
  access_key = var.api_key
  secret_key = var.api_key
}
```
4. HashiCorp Vault: HashiCorp Vault is a tool designed for secret management. Terraform can integrate with Vault to retrieve sensitive information dynamically during execution. This approach provides centralized management and auditability of secrets.

5. External Secret Management Tools: Some organizations use external secret management tools like AWS Secrets Manager, Azure Key Vault, or GCP Secret Manager to store and retrieve sensitive information. Terraform can be configured to interact with these services to obtain secrets dynamically.

6. Encrypted State Files: Encrypting the Terraform state file adds an extra layer of security. You can enable state file encryption by configuring backend settings to use encryption.

----

Q. How can we export data from one module to another?
- In Terraform, exporting data from one module to another can be achieved through `output variables`. Output variables allow you to expose specific values from a module so that they can be used by other modules or in the root configuration.

---
Q. null_resource

- The "null_resource" can be helpful when you need to perform tasks like running local-exec provisioners, calling external scripts, or executing commands that are not related to a particular resource. It provides flexibility and allows for custom actions within the Terraform workflow.

---
Q. 9. What Terraform commands are the most useful?
 
- fmt
- init
- validate
- plan
- apply
- destroy
- output
- show
- state
- version

---
Q. Which value of the TF_LOG variable provides the most verbose logging?
- The most verbose logging level in Terraform is TRACE. This level will log all Terraform messages, including debug messages and provider plugin messages. To set the logging level to TRACE, use the following command:

> TF_LOG=TRACE terraform plan

- The other logging levels, in order of increasing verbosity, are DEBUG, INFO, WARN, and ERROR.