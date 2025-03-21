# **Ansible Playbooks and Become Feature**

## **Introduction to Ansible Playbooks**
Ansible playbooks are a fundamental part of Ansible's orchestration capabilities. They define a set of instructions that Ansible follows to configure and manage systems.

### **Key Concepts of Playbooks**
1. **Playbook:** A YAML file that contains plays.
2. **Play:** Defines a set of tasks to be executed on specified hosts.
3. **Task:** A single action to be performed on a host (e.g., running a command, installing a package).
4. **Modules:** Built-in functionalities in Ansible that perform specific tasks like `command`, `yum`, `service`, etc.

### **Example Playbook Structure**
```yaml
---
- name: Play 1
  hosts: localhost
  tasks:
    - name: Execute command 'date'
      command: date

    - name: Execute script on server
      script: test_script.sh

    - name: Install httpd service
      yum:
        name: httpd
        state: present

    - name: Start web server
      service:
        name: httpd
        state: started
```

## **Understanding the `become` Directive**
### **What is `become`?**
The `become` directive in Ansible is used to **elevate privileges** when running a task that requires administrative access.

### **Why Use `become` If `ansible_user` is Defined?**
- `ansible_user` only defines which user Ansible uses to connect via SSH.
- Some tasks (like installing packages or modifying system files) require root privileges.
- `become: yes` allows Ansible to execute tasks as another user (default: `root`).

### **Example Usage of `become`**
```yaml
- name: Install Nginx
  hosts: web_servers
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
```
This allows Ansible to run the task as root, even if the `ansible_user` is a non-root user.

### **Using `become_user`**
```yaml
- name: Restart PostgreSQL as postgres user
  hosts: db_servers
  become: yes
  become_user: postgres
  tasks:
    - name: Restart PostgreSQL
      service:
        name: postgresql
        state: restarted
```
This will execute the task as the `postgres` user instead of `root`.

## **Check and Diff in Ansible**
### **What is `check` Mode?**
- `ansible-playbook --check playbook.yml` runs the playbook in **dry-run mode**.
- It simulates the changes without applying them.

### **What is `diff` Mode?**
- `ansible-playbook --diff playbook.yml` shows the differences before applying changes.
- Useful for tracking modifications in configuration files.

## **Ansible Lint**
### **What is `ansible-lint`?**
`ansible-lint` is a tool used to check Ansible playbooks for **best practices and style violations**.

### **Running Ansible Lint**
To check a playbook for linting issues:
```sh
ansible-lint playbook.yml
```
This helps in maintaining clean and efficient playbooks.

## **Conclusion**
- Playbooks define automation workflows.
- The `become` directive is used for privilege escalation.
- `check` mode helps in previewing changes.
- `diff` mode shows what will be changed.
- `ansible-lint` ensures adherence to best practices.

By following these guidelines, you can write efficient and well-structured Ansible playbooks.

