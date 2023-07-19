# Hashicorp Packer
---

## Chapter - 2

#### What is a machine image?

a resource that stores all configurations, permissions, and metadata needed to create a virtual machine.

#### Why use packer?

- Create images via template
  - No need to spin up a VM and make changes yourself! Describe your desired machine image, and let Packer generate it.
- Automate all the things
  - Packer fits in nicely with existing CI/CD pipelines. Use it to automatically test new config management formulas or push approved builds to prod.
- Create identical images
  - Cross-cloud, cross-env or cross-platform, a single Packer template can create machine images for multiple use cases

#### Packer Breakdown

```json
{
    "min_packer_version": "1.0.0",
    "variables": {
        "infra_name": "ingrammicrogbs",
        "infra_env": "",
        "aws_region": "ap-southeast-1",
        "aws_instance": "t3.small",
    },
    "builders": [
        {
            //
        },
    ],
    "provisioners": [
        {
          //
        }
    ],
    "post-processors": [
        {
          //
        }
    ],
}
```

**Builder**

Define the desired platform and platform conf, including API key information and desired source images

Define what providers to use as builders (We'll use amazon-ebs to create a AMI we can use for launching ec2 instances)

**Provisioner**

Define how to configure the image, most likely by using your existing configuration management platform. No need to rewrite - just reuse!

Essentially the scripts we run on the server to set everything up the way we want it

**Post-Processer**

Related to the builder; runs after the image is built; generally generates or supplies artifacts

**Communicator**

How packer works on the machine image during creation. By default, *this is SSH and does not need to be defined.*

```json
"builders": [
    {
        "communicator": "winrm",
        "winrm_username": "Administrator",
        "winrm_password": "Secret"
    },
],
```

**Build**

The combination of the previous components to create a machine image; a template can have multiple builds

**Artifact**

The end results of a build; generally, this is the machine image itself and any  generated log or metadata files.

**Command**

The commands run on the CLI to manage Packer; most often, this is the `packer build` command.


---

## Chapter - 7

#### Shell


```json
"provisioners": [
  {
    "type": "shell",
    "inline": ["sudo apt update -y && sudo apt upgrade -y"]
  },
  {
    "type": "shell",
    "scripts": ["init.sh", "script2.sh"] 
  }
]

```

#### File

Upload files and directories to the machine

The trailing slash after a directory dictates how it is uploaded:

- Directories with a slash (*directory/*) upload only the contents of the directory (*/tmp/file.tar*)
- Directories without a slash create a new directory (*/tmp/directory/*\*)


```json
"provisioners": [
  {
    "type": "shell",
    "inline": ["sudo apt update -y && sudo apt upgrade -y"]
  },
  {
    "type": "file",
    "source": "files/",
    "destination": "/tmp"
  }
]

```

#### Ansible

Provision using a remote Ansible server; runs ansible-playbook over SSH


```json
"provisioners": [
  {
    "type": "ansible",
    "playbook_file": "./playbook.yml",
  }
]

```

**ansible-local**

```json
"provisioners": [
  {
    "type": "ansible-local",
    "playbook_file": "playbook.yml",
  }
]
```

---