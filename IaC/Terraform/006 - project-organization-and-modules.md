# What is a Module?

Modules are containers for multiple resources that are used together. A module consists of a collection of **.tf** and/ or **.tf.json** files kept together in a directory.

Modules are the main way to package and reuse resource configurations with Terraform.

# Why modules ?

![Pasted image 20240115113155.png](./.img/Pasted%20image%2020240115113155.png)

In order to let teams know about the certain features and let teams work independently, we split the configurations into modules. We need everyone to know something about the project have a slight idea.

![Pasted image 20240115113136.png](./.img/Pasted%20image%2020240115113136.png)

We can have a module for the database, module for the application, module for the networking, etc.

But in better scenarios, we have **infrastructure specialists** who communicate with the software development team to provision a certain functionality or a feature.

# Types of Modules

- ### Root Module : Default module containing all .tf files in main working directory.
- ### Child Module : A separate external module referred to from a .tf file.

### Module Sources:

|                        |                                  |
| :--------------------: | -------------------------------- |
| **Terraform Registry** | **Generic Git, Mercurial repos** |
|       **GitHub**       | **HTTP URLs**                    |
|    **Local Paths**     | **S3 Buckets**                   |
|     **BitBucket**      | **GCS Buckets**                  |

### Modules Sources

- ##### Local Path

  ![Pasted image 20240119123028.png](./.img/Pasted%20image%2020240119123028.png)

- ##### Terraform Registry

  ![Pasted image 20240119123054.png](./.img/Pasted%20image%2020240119123054.png)

- ##### GitHub
  - ##### HTTPS
  - ##### SSH
- ##### Generic Git Repo

![Pasted image 20240119123145.png](./.img/Pasted%20image%2020240119123145.png)

## Module Blocks

### Module Block

```json
module "module_name" {
    source = "module_source"
    version = "module_version"

    # Input variables
    input_variable_name = "input_variable_value"
}
```

### Module Block with Meta-arguments

```json
module "module_name" {
    source = "module_source"
    version = "module_version"

    # Input variables
    input_variable_name = "input_variable_value"

    # Meta-arguments
    count = 1
    for_each = var.list
    providers = {
        aws = aws
    }
    depends_on = [aws_instance.instance_1]
    lifecycle = {
        create_before_destroy = true
        ignore_changes = [
            "ami",
            "instance_type"
        ]
        prevent_destroy = true
    }
}
```

## Module Calls

```json
module "module_name" {
    source = "module_source"
    version = "module_version"

    # Input variables
    input_variable_name = "input_variable_value"
}
```

## Module Calls with Meta-arguments

```json
module "module_name" {
    source = "module_source"
    version = "module_version"

    # Input variables
    input_variable_name = "input_variable_value"

    # Meta-arguments
    count = 1
    for_each = var.list
    providers = {
        aws = aws
    }
    depends_on = [aws_instance.instance_1]
    lifecycle = {
        create_before_destroy = true
        ignore_changes = [
            "ami",
            "instance_type"
        ]
        prevent_destroy = true
    }
}
```

# Module Composition

![Pasted image 20240119123420.png](./.img/Pasted%20image%2020240119123420.png)

### Module Composition

- Modules can be composed together to create a larger configuration.
- Modules can be reused across multiple configurations.
- Modules can be used to create multiple resources.

#### Module Composition Example

```json
module "module_name" {
    source = "module_source"
    version = "module_version"

    # Input variables
    input_variable_name = "input_variable_value"
}

module "module_name_2" {
    source = "module_source"
    version = "module_version"

    # Input variables
    input_variable_name = "input_variable_value"
}
```

# Module Outputs

- Outputs are used to return values from a module.
- Outputs can be used in other modules.
- Outputs can be used in root modules.

## Module Outputs

```json
output "output_name" {
    value = module.module_name.output_name
}
```

## Module Outputs with Meta-arguments

```json
output "output_name" {
    value = module.module_name.output_name
    description = "description"
    sensitive = true
}
```

# Module Variables

- Variables are used to pass values from root module to child modules.
- Variables can be used in other modules.
- Variables can be used in root modules.

## Module Variables

```json
variable "variable_name" {
    description = "description"
    type = string
    default = "default_value"
}
```

## Module Variables with Meta-arguments

```json
variable "variable_name" {
    description = "description"
    type = string
    default = "default_value"
    sensitive = true
}
```

### Input + Meta-arguments

- Input variables are passed in via module block

#### Meta-Arguments:

- count
- for_each
- providers
- depends_on

![Pasted image 20240119123420.png](./.img/Pasted%20image%2020240119123420.png)

### What makes a good module?

- Raises the abstraction level from base resources types.
- Group resources in a logical fashion
- Exposes input variables to allow necessary customization + composition.
- Provides useful defaults.
- Return outputs to make further integrations possible.
