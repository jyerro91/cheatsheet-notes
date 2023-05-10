# Hashicorp Certified Terraform Associate Foundations

## Chapter - 2

#### IaC and Its Benefits

- No more clicks - Write down what you want to deploy
- Enable DevOps - Codification of deployment means it can be tracked in version control enabling better visibility and collaboration across teams
- Declare your infrastructure - for the majority of cases, written declaratively via code but can be procedural(imperative) too!
- Speed, Cost and Reduced Risk - Less human intervention during deployment means fewew chances of security flaws, superflous resources, and more time is saved!

##### Cloud Agnostic IaC with Terraform

###### Why you'll love it?

1. Automate Software Defined Networking
2. Interacts and takes care of communication with control layer APIs with ease
3. Supports a vast array of private and public cloud vendors
4. Tracks state of each resource deployed

## Chapter - 3

#### Terraform Workflow

- `Write`: This would generally start off with creating a Repo as a common best practice
- `Plan`: You'll continually add and review changes to code in your project
- `Apply`: After one last review/plan, you'll be ready to provision real infrastructure

#### Terraform Init

Initializes the working directory that contains your Terraform code

1. Downloads ancillary components (Downloads modules and plugins)
2. Sets up backend (Sets up the backend for storing Terraform state file, a mechanism by which Terraform tracks resources)

#### Terraform Key Concepts: Plan, Apply and Destroy


##### Reviewing your Terraform Code: Terraform Plan

- Reads the code and then creates and shows a "plan" of execution/deployment
  > Note: This command does not deploy anything. Consider this a read-only command.
- Allows the user to "review" the action plan before executing anything
- At this stage, authentication credentials are used to connect to your infrastructure, if required

##### Deploying your Terraform Code: Terraform Apply

- Deploys the instructions and statements in the code
- Updates the deployment state tracking mechanism file, a.k.a "state file"

##### Cleaning Up Your Terraform Deployment: Terraform Destroy

- Looks at the recorded, stored state file created during deloyment and destroys all resources created by your code
- Should be used with caution, as it is a non-reversible command. Take backups, and be sure that you want to delete infrastructure

#### Resource Addressing in Terraform: Understanding Terraform Code

##### Terraform Code Snippet (Configuring the Provider)

```json
provider "aws" {
  region = "us-east-1"
}
```
**Reserved keyword**

- provider
- resource
- data
- variable



- Terraform executes code in files with the `.tf` ext.
- Initially, Terraform looks for providers in the [Terraform providers registry](https://registry.terraform.io/browse/providers)

## Chapter - 4

#### Installation

**Method 1: Download, Unzip, Use**
**Method 2: Set Up Terraform Repository on Linux(only)**

*Terraform Providers*

- Provders are Terraform way of abstracting with API control layer of the infrastructure vendors.
- Terraform, by defaul, looks for Providers in the Terraform providers registry.
- Provider are `plugins`. They are release on a separate rhythm from Terraform itself, and each provider has its own series of version numbers.
- You can write your own custom providers as well
- Terraform finds and install providers when initializing working directory (via terraform init)
- As a best practice Providers should be pegged down to a specific version, so that any changes across provider version doesn't break your Terraform code.

#### Terraform State: The Concept

*Why State Matters*

**Resource Tracking!**

- A way for Terraform to keep tabs on what has been deployed
- Critical to Terraform functionality
- STored in flat files, by default named "terraform.tfstate"
- Helps Terraform calculate deployment delta and create new deployment plans
- Never lose your Terraform State file!


#### Terraform Variables and Outputs

Can also be define via cli

```bash
terraform apply -var variable_name="value"
```


```json
variable "my-var" {}
// referencing a variable: var.my-var (using . notation)
```
> Note: variable are ideally stored in separate file named terraform.tfvars

##### Validation Feature (optional)

```json
variable "my-var" {
  description = "My Test Variable"
  type = string
  default = "Hello"
  // if you want the value to be hidden during planning
  // default: false
  sensitive = true
  validation {
    condition = length(var.my-var) > 4
    error_message = "The string must be more than 4 characters"
  }
}
// release in version 0.13
// experimental feature 0.12
```

**Base Types:**
- string
- number
- bool

**Complex Types:**
- list, set, map, object, tuple

##### Output Values

```json
output "instance_ip" {
  description = "VM Private IP"
  value = aws_instance.my-vm.private.ip
}
```

- Output variable values are shown on the shell after running `terraform apply`
- Output values are like return values that you want to track after successful Terraform deployment

#### Terraform Provisioners: When to use them

- Terraform way of bootstrapping custom scripts, commands or actions
- Can be run either locally (on the same system where Terraform commands are being issued from), or remotely on resources spun up through the Terraform deploment.


## Chapter - 5

#### Terraform State (State Matters A Lot!)

- Simply put, it maps real-world resources to Terraform configuration
- state is stored locally in a filed called terraform.tfstate
- prior to any modification operation, Terraform refreshes the state file.
- Resource dependency metadata is also tracked via the state file.
- Helps boost deployment performance by caching resource attributes for subsequent use

*Terraform State Command*

- Terraform state command is a utility for manipulating and reading the Terraform State file

Scenario:
- Advance state management
- Manually remove a resource from Terraform State file so that it's not managed by Terraform
- Listing out tracked resources and their details (via state and list subcommands)

#### Local and Remote State Storage

*Local Storage*

- Saves Terraform state locally on your system
- Defaults behavior

*Remote Storage*

- Saves state to a remote data source. Optional Example of storage: AWS S3, Google Storage, Consul
- Allows sharing state file between distributed teams
- Allows locking state so parallel executions don't coincide
- Enables sharing "output" values with other Terraform configuration or code

---

## Chapter - 6

#### Accessing and Using Terraform Modules

*Terraform Modules*

- A module is a container for multiple resources that are used together.
- Every Terraform configuration has at least one module, called the root module, which consists of code files in your main working directory

*AAccessing Terraform Modules*

- Modules can be downloaded or referenced from:
  - Terraform Public Registry
  - A Private Registry
  - Your Local System
- Modules are referenced using a module block
- Other allowed parameters: `count`, `for_each`, `providers`, `depends_on`

```json
// "module" reserved keyword
// "my-vpc-module" module name
module "my-vpc-module" {
  source = "./modules/vpc" // -> Module source (mandatory)
  version = "0.0.5" // -> Module version
  region = var.region // -> Input parameters for module
}
```

*Using Terraform Module*

- Modules can optionally take input and provide outputs to plug back into your main code.

```json
module.module_name.param
```

#### Declaring Module in Code

- Module inputs are arbitrarily named parameters that you pass inside the module block.
- These inputs can be used as variables inside the module code.

*Terraform Module Outputs*

- The outputs declared inside Terraform module code can be fed back into the root module or your main code.

```json
// Output invocation convention in Terraform code
module.<name-of-module>.<name-of-output>
```

*Invoking module output in main code*

```json
// Inside your module code
output "ip_address" {
  value = aws_instance.private_ip
}

module.my-vpc-module.ip_address
```

---

## Chapter - 7

#### Built-In Functions

- Terraform comes pre-packaged with functions to help you transform and combine values.
- User-defined funcitons are not allowed - only built-in ones
- General syntax: function(arg1, arg2)
- Built-in functions are extremely useful in making Terraform code dynamic and flexible

```json
join("-", ["terraform", var.project-name])
```
- A huge array of useful functions can be found
- file
- max
- flatten

```bash
# to test terraform built-in functions
terraform console
```

#### Type Constraints

- Type constraints control the type of variable values.
- `Primitive`: Single type value
  - number
  - string
  - bool
- `Complex`: multiple types in a single variable
  - list
  - tuple
  - map
  - object



#### Complex Types - Collections

- **Collection** types allow multiple values of one primitive type to be grouped together
- Constructors for these Collections include:
  - list(type)
  - map(type)
  - set(type)

#### Complex Types - Structural

- **Structural** types allow multiple values of different primitive types to be grouped together
- Contructors for these Collections include:
  - object(type)
  - tuple(type)
  - set(type)

#### Dynamic Types - The 'any' constraint

- `any` is a placeholder for a primitive type yet to be decided
- Actual type will be determined in runtime

```json
// Example
variable "data" {
 type = list(any)
 default = [ 1 , 3, 52 ]
}
// Terraform recognizes all values as numbers in one variable
```

#### Dynamic Blocks

- Dynamically constructs repeatable nested configuration blocks inside Terraform resources
- Supported within the following block types:
  - resource
  - data
  - provider
  - provisioner
- Used to make your code look cleaner
- 

---

## Chapter - 8

#### Terraform fmt, taint, and import commands

**Terraform fmt (format)**

- Formats Terraform code for readability
- Helps in keeping code consistent
- Safe to run at any time

```bash
terraform fmt
```

*Scenario*

- Before pushing your code to version control
- After upgrading Terraform or its modules
- Any time you've made changes to code

**Terraform taint**

- Tains a resource, forcing it to be destroyed and recreated
- Modifies the state file, which causes the recreation workflow
- Tainting a resource may cause other resources to be modifies

```bash
terraform taint RESOURCE_ADDRESS
```

*Scenario*

- To cause provisioners to run.
- Replace misbehaving resources forcefully
- To mimic side effects of recreation not modeled by any attributes of the resource

**Terraform import**

- Maps existing resources to Terraform using an "ID"
- "ID" is dependent on the underlying vendor, for example to import an AWS EC2 instance you'll need to provide its instance ID
- Importing the same resource to multiple Terraform resources can cause unknown behavior and is not recommended.

```bash
terraform import Resource_address ID
```

*Scenario*

- When you need to work with existing resources
- Not allowed to create new resources
- When you're not in control of creation process of infrastructure

**Terraform Configuration Block**

- A special configuration block for controlling Terraforms own behavior.
- This block only allows constant values, named resources and variables are not allowed in it.

*Example*

- Configuring backend for storing state files
- Specifying a required Terraform version
- Specifying a required Terraform Provider version and its requirements
- Enable and test Terraform experimental features
- Passing metadata to providers

```json
terraform {
  required_version = ">=0.13.0"
  required_providers {
    aws = ">=3.0.0"
  }
}
```

#### Terraform Workspaces (CLI)

- The Terraform Workspaces are alternate state files within the same working directory
- Terraform starts with a single workspace that is always called `default`. It cannot be deleted.
- Workspaces are meant to share resources and to help enable collaboration
- Access to a Workspace name is provided through the `${terraform.workspace}` variable


```bash
# Create a workspace
terraform workspace new WORKSPACE-NAME

# Select a workspace
terraform workspace select WORKSPACE-NAME

```

*Scenario*

- Test changes using a parallel, distinct copy of infrastructure
- IT can be modeled against branches in version control such as GIT


#### Debugging Terraform ( TF_LOG and TF_LOG_PATH )

- `TF_LOG` is an environment variable for enabling verbose logging in Terraform. By default, it will send logs to stderr (standard error output)
- Can be set to the following levels: `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`
- `TRACE` is the most verbose level of logging and the most reliable one
- To persist logged output, use the `TF_LOG_PATH` environment variable
- Setting logging environment variables for Terraform on Linux:

```bash
export TF_LOG=TRACE
export TF_LOG_PATH=./terraform.log
```

---

## Chapter - 9

#### Benefits of Sentinel (Enterprise)

- Enforces policies on your code
- Has its own policy language - Sentinel Language
- Designed to be approachable by non-programmers

**Benefits**

- Sandboxing - Guardrails for automation
- Codification - Easier understanding, better collaboration
- Version Control
- Testing and Automation

**Use cases**

- For enforcing CIS standards across AWS accounts
- Checking to make sure only t3.micro instance types are used
- Ensuring Security Groups do not allow traffic on port 22

#### Terraform Vault

- Secrets management software
- Dynamically provisions credentials and rotates them
- Encrypts sensitive data in transit and at rest and provides fine-grained access to serets using ACLs

**Benefits**

- Developers don't need to manage long-lived credentials
- Inject secrets into your Terraform deployment at runtime
- Fine-grained ACLs for access to temp credentials

#### Terraform Registry and Cloud Workspaces

**Registry**

- A repository of publicly availble Terraform providers and modules
- You can publish and share your own modules, too
- You can collaborate with other contributors to make changes to providers and modules
- can be directly referenced in your Terraform code

**Cloud Workspaces**

- Directories hosted in Terraform Cloud!
- Stores old versions of state files by default
- Maintains a record of all execution activity
- All Terraform commands executed on "managed" Terraform Cloud VMs



---
