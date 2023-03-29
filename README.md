# Ansible

- [Getting Started](https://docs.ansible.com/ansible/latest/getting_started/index.html)

# Install

```bash
$ python3 -m pip install --user ansible
```

# Inventory

- [How to build inventory](https://docs.ansible.com/ansible/latest/getting_started/get_started_inventory.html#get-started-inventory)

Create an inventory file

```bash
# inventory.yml

mymachinesgroup:
  hosts:
    vm01:
      ansible_host: 192.0.2.50
    vm02:
      ansible_host: 192.0.2.51
    vm03:
      ansible_host: 192.0.2.52
```

Verify your inventory

- If you created your inventory in a directory other than your home directory, specify the full path with the -i option

```bash
$ ansible-inventory -i inventory.yaml --list

# if inventory organized in a directory called `inventory`
$ ansible-inventory -i inventory/ --list
```


Ping the managed nodes in your inventory
(In this example, the group name is `mymachinesgroup` which you can specify with the ansible command instead of `all`)

```bash
$ ansible mymachinesgroup -m ping -i inventory.yaml

# if inventory organized in a directory called `inventory`
$ ansible mymachinesgroup -m ping -i inventory/
```

```
vm03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
vm02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
vm01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```


# Playbook

- [Creating a Playbook](https://docs.ansible.com/ansible/latest/getting_started/get_started_playbook.html#get-started-playbook)
- [Working with Playbooks](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks.html#working-with-playbooks)
- [Reusing Playbooks](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse.html#re-using-playbooks)


Playbooks are automation blueprints, in YAML format, that Ansible uses to deploy and configure managed nodes.

A list of plays that define the order in which Ansible performs operations, from top to bottom, to achieve an overall goal.

## Play

An ordered list of tasks that maps to managed nodes in an inventory.

## Task

A list of one or more modules that defines the operations that Ansible performs.

## Module

A unit of code or binary that Ansible runs on managed nodes. Ansible modules are grouped in collections with a Fully Qualified Collection Name (FQCN) for each module.


A Sample Playbook:

```yaml
# playbook.yml

playbook.yaml:

- name: My first play
  hosts: virtualmachines
  tasks:
   - name: Ping my hosts
     ansible.builtin.ping:
   - name: Print message
     ansible.builtin.debug:
       msg: Hello world
```

Run your playbook:

```bash
$ ansible-playbook -i inventory.yaml playbook.yaml

# if inventory & playbook is organized in a directory
$ ansible-playbook -i inventory/ playbook/lan.yaml
```

```bash
PLAY [My first play] **********************************************************************

TASK [Gathering Facts] ********************************************************************
ok: [vm01]
ok: [vm02]
ok: [vm03]

TASK [Ping my hosts] **********************************************************************
ok: [vm01]
ok: [vm02]
ok: [vm03]

TASK [Print message] **********************************************************************
ok: [vm01] => {
    "msg": "Hello world"
}
ok: [vm02] => {
    "msg": "Hello world"
}
ok: [vm03] => {
    "msg": "Hello world"
}

PLAY RECAP ********************************************************************************
vm01: ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
vm02: ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
vm03: ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## Role

- [Role directory structure](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html#role-directory-structure)
- [Using Roles](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html#role-directory-structure)
    - Mentioning roles
    - vs Dynamically including/improting roles (this is done through tasks)
