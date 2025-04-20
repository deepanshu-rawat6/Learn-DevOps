# 004 - ansible inventory

## introduction

For `ansible` to connect to host it uses: SSH for Linux and Powershell Remoting for Windows

It stores all of the information in an inventory files, located at `/etc/ansible/hosts`.

### inventory

`inventory` file is in a format of `.ini` file like format.

```ini
server1.org.com
server2.org.com

[mail]
server3.org.com
server4.org.com
```

We can arrange the files in this order as above.

Also, we can create `groups` by using `[<group-name>]` and then listing our servers

### more on inventory files

So, if we want to specify which sever is associated to which context, then we can add in an `alias`.  `ansible_host` is the inventory parameter which takes either the server IP or FQDN(Fully Qualified Domain Name)

**Note**: FQDN

https://www.mydomain.com 

Protocol: https://
Subdomain: www
Domain name: mydomain
Top-level domain: com
Root domain: mydomain.com

Use SSH-key based passwordless authentication

```ini
web  ansible_host=server1.org.com  ansible_connection=ssh  ansible_user=root
db   ansible_host=server2.org.com  ansible_connection=winrm ansible_user=admin
mail ansible_host=server3.org.com  ansible_connection=ssh
web2 ansible_host=server4.org.com  ansible_connection=winrm

localhost ansible_connection=localhost
```

More parameters:
* ansible_connection - ssh / winrm / localhost 
* ansible_port - 22 / 5986
* ansible_user - root / administrator
* ansible_ssh_pass - Password

Security - ansible vault

### inventory formats

`ini` and `yaml`

### group, parents and children

```ini
web  ansible_host=server1.org.com  ansible_connection=ssh  ansible_user=root
db   ansible_host=server2.org.com  ansible_connection=winrm ansible_user=admin
mail ansible_host=server3.org.com  ansible_connection=ssh
web2 ansible_host=server4.org.com  ansible_connection=winrm

localhost ansible_connection=localhost

[web_nodes]
web
web2

[db_nodes]
db

[mail_nodes]
mail

[infra:children]
web_nodes
db_nodes
mail_nodes
```

```yaml
all:
	children:
		webservers:
			children:
				webservers_us:
					hosts:
						server1_us.com:
							ansible_host: 192.168.8.101
						server2_us.com:
							ansible_host: 192.168.8.101
				webservers_eu:
					hosts:
						server1_eu.com:
							ansible_host: 10.168.8.101
						server2_eu.com:
							ansible_host: 10.168.8.101
```