# Expressions + Functions

## Expressions

- Template strings
- Operators (! , - , \* , / , % , > , == , etc..)
- Conditionals ( cond ? true : false)
- For ( [ for o in var.list : o.id] )
- Splat ( var.list[ * ].id )
- Dynamic Blocks
- Constraints ( Type & Version )

## Functions

- Numeric
- String
- Collection
- Encoding
- Filesystem
- Date & Time
- Hash & Crypto
- IP Network
- Type Conversion

## Meta-Arguments

### depends_on

![Pasted image 20240115100528.png](./.img/Pasted%20image%2020240115100528.png)

- Terraform automatically generates dependency graph based on references.
- If two resources depend on each other (but not each others data), _depends_on_ specifies that dependency to enforce ordering.
- For example, if software on the instance needs access to S3, trying to create the _aws_instance_ would fail if attempting to create it before the _aws_iam_role_policy_ .

### Count

![Pasted image 20240115100547.png](./.img/Pasted%20image%2020240115100547.png)

- Allows for creation of multiple resources/modules from a single block.
- Useful when the multiple necessary resources are nearly identical.

### for_each

![Pasted image 20240115100600.png](./.img/Pasted%20image%2020240115100600.png)

- Allows for creation of multiple resources/modules from a single block.
- Allows more control to customize each resource than _count_ .

### Lifecycle

![Pasted image 20240115100748.png](./.img/Pasted%20image%2020240115100748.png)

- A set of meta arguments to control terraform behavior for specific resources.
- _create_before_destroy_ can help with zero downtime deployments.
- _ignore_changes_ prevents Terraform from trying to revert metadata being set elsewhere.
- _prevent_destroy_ causes Terraform to reject any plan which could destroy this resource.

## Provisioners

Provisioners, perform actions on local or remote machines

- file
- local-exec
- remote-exec
- vender
  - chef
  - puppet
