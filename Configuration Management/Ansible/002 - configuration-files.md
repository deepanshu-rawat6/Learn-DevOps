# ansible configuration files

Default Path: `/etc/ansible/ansible.cfg`

```yaml
[defaults]

inventory      = /etc/ansible/hosts
log_path       = /var/log/ansible.log

library        = /usr/share/my_modules/
roles_path     = /etc/ansible/roles
action_plugins = /usr/share/ansible/plugins/action

gathering      = implicit

#ssh timeout
timout         = 10
forks          = 5

[inventory]

enable_plugins = host_list, virtualbox, yaml, constructed

[privilege_escalation]

[paramiko_connection]

[ssh_connection]

[persistent_connection]

[colors]
```

## configs example

So, ansible will use the values provided in this config files for any `playbook`, in our local machine.

Say, I have three separate folders for playbooks for:

* `/opt/web-playbooks`
* `/opt/db-playbooks`
* `/opt/network-playbooks`

But, now I want different configs for these specific playbooks. Then, we can make the a separate copy of our config for each playbook, and can overwrite our requirements in those configs.

* `/opt/web-playbooks/ansible.cfg
* `/opt/db-playbooks/ansible.cfg`
* `/opt/network-playbooks/ansible.cfg`

Or, even better we can still keep our config in `/etc/ansible`, but copy the required config as `/etc/ansible-web.cfg`.

```zsh
$ANSIBLE_CONFIG=/opt/ansible-web.cfg ansible-playbook playbook.yml
```

## precedence of config files

1. Files with path linked with $ANSIBLE_CONFIG
2. Files in local folders(`web` -> `/opt/web/ansible.cfg)
3. Users directory (`.ansible.cfg`)
4. Main file (`/etc/ansible/ansible.cfg`)

**Note** : We just need to overwrite the parameters we need to change for a specific playbook, rest will be picked from playbooks down the line.

## more on env variables

Say, now we want a playbook for `opt/storage-playbooks`, we have not mentioned our config file for it, so by default one at `/etc/ansible/ansible.cfg` will be used.

But, say we want to change a param:

```
gathering = implict
```

Instead of creating a config files, we can set the env variable for that specific config

**Note**: General norm, if parameter `gathering`, so env variable -> `ANSIBLE_GATHERING=explicit`. Use `export` to set the variable.

Command, now:

```zsh
$ ANSIBLE_GATHERING=explicit ansible-playbook playbook.yml
```

To use it in a shell:

```zsh
$ export ANSIBLE_GATHERING=explicit 
$ ansible-playbook playbook.yml
```

But, to use it for a broader scale, saving a different config with the overridden parameters is the best solution.

## view configurations

To list all configurations

```zsh
$ ansible-config list 
```

To view the current config

```zsh
$ ansible-config view
```

Shows the current settings (where the changes are picked from....)

```zsh
$ ansible-config dump
```


