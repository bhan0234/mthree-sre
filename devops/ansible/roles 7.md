# Ansible Roles

## Introduction to Roles

Roles in Ansible help in organizing and reusing automation code. Just as people have roles in real life—such as doctors, engineers, or chefs—servers in an IT environment also have roles, such as database servers, web servers, or cache servers. Assigning a role in Ansible means executing all the necessary tasks to configure a server for a specific purpose.

For example, making a server a MySQL database server involves:

- Installing prerequisites
- Installing MySQL
- Configuring MySQL services
- Creating databases and users

## Why Use Roles?

Roles help:

1. **Reuse automation code** – Instead of rewriting similar tasks for multiple projects, you can package them into reusable roles.
2. **Organize your playbooks** – A role has a structured directory layout that keeps tasks, handlers, templates, and variables neatly arranged.
3. **Share automation scripts** – Roles can be shared within a team or the larger Ansible community via Ansible Galaxy.

## Ansible Role Directory Structure

Ansible enforces a specific directory structure for roles:

```
my_role/
├── defaults/        # Default variables
│   └── main.yml
├── files/          # Static files to be copied
├── handlers/       # Handlers (e.g., restarting services)
│   └── main.yml
├── meta/           # Role metadata
│   └── main.yml
├── tasks/          # Main tasks to execute
│   └── main.yml
├── templates/      # Jinja2 templates
├── tests/          # Test cases
├── vars/           # Additional variables
│   └── main.yml
```

Each directory serves a purpose, making roles modular and easy to maintain.

## Creating a Role

Instead of manually creating the directory structure, Ansible provides a command to initialize a role:

```sh
ansible-galaxy init my_role
```

This generates the necessary files and folders under `my_role/`.

## Using Roles in a Playbook

Once a role is created, it can be assigned to hosts within a playbook:

```yaml
- name: Configure MySQL Server
  hosts: db_servers
  roles:
    - my_role
```

This tells Ansible to execute all tasks within `my_role` on `db_servers`.

### Role Paths and Locations

Ansible looks for roles in:

1. A `roles/` directory within the playbook location.
2. The system-wide `/etc/ansible/roles/` directory.
3. Any custom paths set in `ansible.cfg`.

To check the default role paths:

```sh
ansible-config dump | grep ROLE
```

## Downloading and Using Roles from Ansible Galaxy

Ansible Galaxy is a public repository where users share roles. To search for roles:

```sh
ansible-galaxy search nginx
```

To install a role:

```sh
ansible-galaxy install geerlingguy.nginx
```

By default, roles are installed in `/etc/ansible/roles/`, but you can specify a custom path using `-p`:

```sh
ansible-galaxy install -p ./roles geerlingguy.nginx
```

## Assigning Multiple Roles to Hosts

A single host can have multiple roles:

```yaml
- name: Setup Web Server
  hosts: web_servers
  roles:
    - nginx
    - mysql
```

This configures both Nginx and MySQL on `web_servers`.

## Passing Variables to Roles

Roles can accept variables for customization. Variables can be passed like this:

```yaml
- name: Setup Database
  hosts: db_servers
  roles:
    - role: mysql
      become: yes
      vars:
        mysql_root_password: "securepassword"
```

## Managing Installed Roles

To list installed roles:

```sh
ansible-galaxy list
```

To remove a role:

```sh
ansible-galaxy remove geerlingguy.nginx
```

## Conclusion

Roles are a powerful way to modularize, reuse, and share Ansible automation. By structuring tasks, variables, templates, and handlers into well-organized directories, roles simplify complex automation workflows. Using Ansible Galaxy, teams can leverage community roles, reducing effort and improving efficiency.

