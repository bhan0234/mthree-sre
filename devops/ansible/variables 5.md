# Ansible Variables: A Detailed Guide

## What Are Variables in Ansible?

Just like in any other scripting or programming language, **variables** in Ansible are used to store values that can vary across different hosts, tasks, or environments.

For example, if you need to apply patches to hundreds of servers, a single playbook can be used for all of them. However, the **hostnames, usernames, or passwords** will differ per server, and this information can be stored in **variables**.

## Defining Variables in Ansible

### 1. Variables in the Inventory File
Variables can be defined inside an **inventory file** using key-value pairs. For example:

```ini
[all:vars]
ansible_host=192.168.1.100
ansible_user=admin
ansible_password=securepassword
```

Here, `ansible_host`, `ansible_user`, and `ansible_password` are variables.

### 2. Variables in the Playbook
Variables can also be defined **inside a playbook** using the `vars` directive:

```yaml
---
- hosts: all
  become: yes
  vars:
    dns_server: "8.8.8.8"
  tasks:
    - name: Add DNS entry
      lineinfile:
        path: /etc/resolv.conf
        line: "nameserver {{ dns_server }}"
```

Here, `dns_server` is defined as a variable and used inside `lineinfile`.

### 3. Using Variables in a Separate File
For better organization, variables can be placed in a **separate file** and included in the playbook. This is useful in large-scale configurations.

Example:

Create a file `vars.yaml`:
```yaml
dns_server: "8.8.8.8"
gateway: "192.168.1.1"
```

Include it in the playbook:
```yaml
---
- hosts: all
  become: yes
  vars_files:
    - vars.yaml
  tasks:
    - name: Configure network
      lineinfile:
        path: /etc/network/interfaces
        line: "gateway {{ gateway }}"
```

### 4. Host-Specific Variables
Host-specific variables can be stored in **host variable files** under the `host_vars/` directory:

```yaml
# File: host_vars/webserver.yaml
dns_server: "8.8.8.8"
```

Now, when running the playbook for `webserver`, it will automatically load the variables from `host_vars/webserver.yaml`.

### 5. Group-Specific Variables
Similarly, variables can be grouped under `group_vars/` for specific host groups:

```yaml
# File: group_vars/webservers.yaml
timezone: "UTC"
```

These values will be available for all hosts in the `webservers` group.

## Using Variables in Playbooks

To reference a variable in an Ansible playbook, use **double curly braces** (`{{ variable_name }}`).

Example:
```yaml
- name: Install applications
  yum:
    name: "{{ app_list }}"
    state: present
```

## Example: Dynamic List of Packages

Previously, we hardcoded the package list in the playbook:

```yaml
- name: Install applications
  yum:
    name:
      - vim
      - sqlite
      - jq
    state: present
```

Instead, we can use the `app_list` variable from the inventory file:

```yaml
- hosts: all
  become: yes
  tasks:
    - name: Install applications
      yum:
        name: "{{ app_list }}"
        state: present
```

And define `app_list` in the inventory file:
```ini
[all:vars]
app_list=["vim", "sqlite", "jq"]
```

## Jinja2 Templating in Ansible

Ansible uses **Jinja2 templating** to process variables. Here’s how to use it properly:

1. Enclose variables in double curly braces: `{{ variable_name }}`
2. If starting an assignment with a variable, enclose it in quotes: `"{{ variable_name }}"`
3. If the variable is inside a sentence, no additional quotes are needed:
   ```yaml
   message: "Your assigned IP is {{ ip_address }}."
   ```

## Summary

- Variables in Ansible allow dynamic configurations.
- They can be defined in **inventory files, playbooks, or separate files**.
- Variables can be specific to **hosts or groups**.
- **Jinja2 templating** is used to reference variables in playbooks.
- Using **variables** improves playbook **reusability and flexibility**.

By leveraging variables effectively, we can write **scalable and maintainable** Ansible playbooks.

