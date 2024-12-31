# Variable Types

## Input Variables

### Referenced by : var.{name}

Input variables are like a input parameters for a function.

```json
variable "instance_type" {
  description = "value of instance type"
  type = string
  default = "t2.micro"
}
```

## Local Variables

### Referenced by : local.{name}

Temporary variables within the scope of the function, to can be pulled into a variable to be reused.

```json
locals {
    service_name = "ABC"
    owner = "Sehnsucht"
}
```

## Output Variables

### Referenced by :

Output variables are like a combination of both of the above. We can bundle multiple terraform config files. These variables are logged out.

```json
output "instance_ip_addr" {
    value = aws_instance.instance_1.public_ip
}
```

## Setting Input Variables

(In order of precedence // lowest -> highest)

- Manual entry during plan/apply
- Default value in declaration block
- TF*VAR*{name} environment variables
- terraform.tfvars file
- _.auto.tfvars_ file
- Command line -var or -var file

## Types & Validation

### Primitive Types:

- string
- number
- bool

### Complex Types:

- list({TYPE})
- set({TYPE})
- map({TYPE})
- object({<> = <>, ...})
- tuple([<>], ...)

### Validation

- Type checking happens automatically.
- Custom conditions can also be enforced.

## Sensitive Data

#### Mark variables as sensitive:

- Sensitive = true

#### Pass to terraform apply with:

- TV_VAR_variable
- -var (retrieved from secret manager at runtime)

#### Can also use external secret store

- For example, AWS Secrets Manager

## Some important points:

- We are using var.{name} to access the variables defined in variables.tf file.
- If the variable are not sensitive then we can use the default value as well, which is defined in terraform.tfvars file.
- By default, terraform.tfvars and .auto.tfvars files are automatically loaded.
- Inorder to load the variables from the file which is not named as terraform.tfvars or .auto.tfvars, we need to use -var-file flag.

Some examples of setting sensitive data:

```bash
terraform apply -var="db_user=myuser" -var="db_pass=something"
```
