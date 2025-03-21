# Conditionals in Ansible

## 1. Using `when` Condition
The `when` keyword is used to conditionally run a task based on a condition.

### Example: Install Apache only on RedHat-based systems
```yaml
- name: Install Apache on RedHat-based systems
  yum:
    name: httpd
    state: present
  when: ansible_os_family == "RedHat"
```

## 2. Using Variables in Conditions
You can use variables to make conditionals dynamic.

### Example: Restart a service only if a variable is set to true
```yaml
- name: Restart service
  service:
    name: apache2
    state: restarted
  when: restart_service | default(false)
```

## 3. Multiple Conditions (`and`, `or`, `not`)

### Example: Install Apache only on Ubuntu with 4GB+ RAM
```yaml
- name: Install Apache
  apt:
    name: apache2
    state: present
  when: ansible_os_family == "Debian" and ansible_memtotal_mb > 4000
```

### Example: Restart service if running OR config changed
```yaml
- name: Restart Apache if running or config changed
  service:
    name: apache2
    state: restarted
  when: service_running or config_changed
```

### Example: Exclude a specific OS
```yaml
- name: Do not run on CentOS
  debug:
    msg: "This task will not run on CentOS!"
  when: ansible_distribution != "CentOS"
```

## 4. Conditionals with Lists (`in`)

### Example: Install package only if OS is Ubuntu or Debian
```yaml
- name: Install package
  apt:
    name: nginx
    state: present
  when: ansible_distribution in ["Ubuntu", "Debian"]
```

## 5. Checking Command Output in Conditions
Use `register` to store a command’s output and use it in `when`.

### Example: Restart Nginx only if it's not running
```yaml
- name: Check if Nginx is running
  shell: systemctl is-active nginx
  register: nginx_status
  ignore_errors: yes

- name: Start Nginx if not running
  service:
    name: nginx
    state: started
  when: nginx_status.stdout != "active"
```

## 6. Using Facts for Conditional Execution
Ansible gathers system facts that you can use in conditions.

### Example: Set up a directory only on AWS instances
```yaml
- name: Create directory for AWS instances
  file:
    path: /opt/aws_data
    state: directory
  when: ansible_ec2_instance_id is defined
```

## 7. Skipping Hosts Dynamically (`failed_when` and `changed_when`)

### Example: Fail if a file doesn’t exist
```yaml
- name: Check if file exists
  stat:
    path: /etc/myconfig.conf
  register: file_stat
  failed_when: not file_stat.stat.exists
```

### Example: Ignore error if service is already stopped
```yaml
- name: Stop Apache
  service:
    name: apache2
    state: stopped
  ignore_errors: yes
  failed_when: "not-found" in ansible_failed_result.stderr
```

## Using `when` with Loops

You can conditionally execute loop iterations using the `when` directive.

### Example: Installing Only Required Packages
```yaml
- name: Install required software
  hosts: all
  become: yes
  vars:
    packages:
      - name: nginx
        required: True
      - name: mysql
        required: True
      - name: apache
        required: False
  tasks:
    - name: Install required packages
      apt:
        name: "{{ item.name }}"
        state: present
      when: item.required == True
      loop: "{{ packages }}"
```
In this example, only `nginx` and `mysql` are installed since they have `required: True`.

## Summary
| Feature            | Example |
|--------------------|---------|
| **Basic `when`**  | `when: ansible_os_family == "Debian"` |
| **Using Variables** | `when: restart_service == true` |
| **Multiple Conditions** | `when: os == "Ubuntu" and ram > 4000` |
| **Lists (`in`)** | `when: ansible_distribution in ["Ubuntu", "Debian"]` |
| **Checking Command Output** | `when: nginx_status.stdout != "active"` |
| **Using Facts** | `when: ansible_ec2_instance_id is defined` |
| **Handling Errors** | `failed_when: not file_stat.stat.exists` |

## Practical Exercises
- Modify your playbook to **only install packages on Ubuntu**.
- Use a registered variable to **restart a service only if it's running**.
- Use `in` to **check multiple OS types**.



# Ansible Loops: A Detailed Guide

## Introduction to Loops in Ansible
Loops in Ansible allow you to execute a task multiple times with different values, eliminating the need for repetitive code. This makes your playbooks more efficient and maintainable.

## Example of a Basic User Creation Task
Imagine a scenario where you need to create multiple users using the `user` module. Without loops, you would need to duplicate the task for each user, which is inefficient. Instead, we can use loops to iterate over a list of users.

### Without Loops (Inefficient)
```yaml
- name: Create multiple users
  hosts: localhost
  become: yes
  tasks:
    - name: Create user Joe
      user:
        name: joe
        state: present

    - name: Create user George
      user:
        name: george
        state: present

    - name: Create user Ravi
      user:
        name: ravi
        state: present
```

### With Loops (Efficient)
```yaml
- name: Create multiple users using loops
  hosts: localhost
  become: yes
  vars:
    users:
      - joe
      - george
      - ravi
  tasks:
    - name: Create users
      user:
        name: "{{ item }}"
        state: present
      loop: "{{ users }}"
```

### How Loops Work
- `loop` is a directive that executes the same task multiple times, iterating over each item in a list.
- The current item in the loop is stored in the `item` variable and can be accessed using `{{ item }}`.
- This makes playbooks more readable and reduces duplication.

## Working with Arrays of Dictionaries
Sometimes, each user may have additional attributes such as user ID. In such cases, we use a list of dictionaries instead of a simple list.

```yaml
- name: Create users with UID
  hosts: localhost
  become: yes
  vars:
    users:
      - { name: 'joe', uid: 1001 }
      - { name: 'george', uid: 1002 }
      - { name: 'ravi', uid: 1003 }
  tasks:
    - name: Create users with UID
      user:
        name: "{{ item.name }}"
        uid: "{{ item.uid }}"
        state: present
      loop: "{{ users }}"
```

- Since `item` is now a dictionary, we use `item.name` and `item.uid` to access its properties.

## Using `with_items` (Older Syntax)
In older versions of Ansible, `with_items` was used instead of `loop`:
```yaml
- name: Create users using with_items
  hosts: localhost
  become: yes
  vars:
    users:
      - joe
      - george
      - ravi
  tasks:
    - name: Create users
      user:
        name: "{{ item }}"
        state: present
      with_items: "{{ users }}"
```
- `loop` is now preferred over `with_items`, but you may still encounter `with_items` in legacy playbooks.

## Advanced `with_*` Directives
While `with_items` iterates over a list of items, other `with_*` directives help perform specific tasks:

| Directive | Purpose |
|-----------|---------|
| `with_file` | Reads content from multiple files |
| `with_url` | Fetches content from multiple URLs |
| `with_lines` | Reads multiple lines from a command output |
| `with_dict` | Iterates over key-value pairs in a dictionary |
| `with_inventory_hostnames` | Loops over hostnames in the inventory |

### Example: `with_file`
```yaml
- name: Read multiple files
  hosts: localhost
  tasks:
    - name: Read content from files
      debug:
        msg: "{{ lookup('file', item) }}"
      with_file:
        - /etc/hosts
        - /etc/passwd
```

### What Are Lookup Plugins?
- Lookup plugins are custom scripts that retrieve data from external sources.
- `with_*` directives are built on lookup plugins.
- Examples include fetching data from files, databases, or external APIs.

## Conclusion
- **Loops** simplify repetitive tasks in Ansible playbooks.
- Use **`loop`** for simple iteration and **`with_*` directives** for advanced use cases.
- **`loop` is preferred** over `with_items` in modern Ansible.
- **Lookup plugins** power `with_*` directives, allowing integration with external sources.

By using loops effectively, you can create scalable and maintainable playbooks for automation in Ansible!



