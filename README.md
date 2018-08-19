# Network Automation Study Grp

* Python
* Ansible
* Linux
* Git
* CICD

# ANSIBLE

# What is Ansible?

Ansible is a powerful IT automation tool that's easy to learn. It's simple 
enough for everyone in your IT team to use, yet powerful enough to automate even 
the most complex deployments. Ansible handles repetitive tasks, giving your team 
more time to focus on innovation.

Designed for multi-tier deployments since day one, Ansible models your IT 
infrastructure by describing how all of your systems inter-relate, rather than just 
managing one system at a time.

Ref: [Getting Started](https://www.ansible.com/get-started)
Ref: [Developer Docs](http://docs.ansible.com/ansible/index.html)

# Why Ansible?

* Automate infrastructure deployment and configuration management
* Simple and easy to learn and easy to get started
* Agentless
* Amazing community
* Excellent documentation
* Lot of Network Modules

# Ansible - Key Concepts

Ansible scripts are written in YAML and it needs no remote agent. 
All managed nodes (routers, switches, firewalls, etc.,) require only SSH access.

### Declarative

It defines the final state rather than describing the sequence of steps necessary 
to achieve that state. i.e. You tell it what you want, not how to do it.

### Idempotent
 
Operations are idempotent if the result of performing it once is exactly the same 
as performing it repeatedly.
 
### Inventory

The ansible inventory is a file that describes Hosts and Host Groups.

```
[routers]
r1
r2

[switches]
sw1
sw2

[all:children]
routers
switches
```

### Playbooks and Tasks

A `playbook` is basically a script used to combine multiple plays. 
A `play` is minimally a mapping between a set of hosts selected by a host specifier 
and the tasks and modules that run on those hosts.

```
- name: Collect router information
  hosts: routers

  tasks:
  - name: Run IOS facts
    ios_facts:

```

### Facts

Facts are things that are automatically discovered on start up about remote nodes.
While they can be used in playbooks and templates just like variables, facts are 
things that are inferred, rather than set. 

Run the setup module to see all facts.
```
$ ansible localhost -m setup
```

Playbook example
```
- hosts: localhost
  connection: local

  tasks:
    - name: log current date as debug message
      debug: msg="the current system date is {{ ansible_date_time.iso8601 }}"
```

Output
```
$ ansible-playbook facts.yml 

PLAY [localhost] *************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************
ok: [localhost]

TASK [log current date as debug message] *************************************************************************************
ok: [localhost] => {
    "msg": "the current system date is 2017-06-28T11:10:10Z"
}

PLAY RECAP *******************************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0   

```

### Variables

As opposed to Facts, variables are simple values (integers, booleans, strings) 
or complex ones (dictionaries, lists, etc) that can be used in templates and playbooks. 
They are declared. i.e. not things that are inferred from the remote system’s current state. 

Variables can be applied to all hosts, host groups or individual hosts
or roles. They can be defined in inventory, playbooks, command line arguments
and other config files.

```
- hosts: webservers
  vars:
    remote_config_path: /etc/foo-service/foo.conf
  
  tasks:   
  - name: install config file
    template: src=foo.conf.j2 dest={{ remote_config_path }}
```

### Modules

Modules control system resources, like services, packages, or files 
or handle executing ad-hoc system commands. They're basically the stuff that does all 
the heavy lifting. Modules can be written in Python, Perl, Bash, or Ruby.

Ansible ships with a massive number of core modules and even more user contributed
modules. Generally speaking, if you can think of it, there's already a module for it.

Ref: [Modules](http://docs.ansible.com/ansible/modules_by_category.html)

### Roles

Roles are units of organization in Ansible. Think re-usable playbooks. 
Assigning a role to a host or group of hosts implies that they should implement that behavior.

```
- hosts: all
  roles:
    - ce_router
    - core_switch
    - access_swtich
     
```

### Templates

Ansible uses Jinja2 templating to enable dynamic expressions and access to variables
with file templates.

```
<!DOCTYPE html>
<html lang="en">
<head>
    <title>My Webpage</title>
</head>
<body>
    <ul id="navigation">
    {% for item in navigation %}
        <li><a href="{{ item.href }}">{{ item.caption }}</a></li>
    {% endfor %}
    </ul>

    <h1>My Webpage</h1>
    {{ a_variable }}

    {# a comment #}
</body>
</html>
```

Ref: [Template Module](http://docs.ansible.com/ansible/template_module.html)

