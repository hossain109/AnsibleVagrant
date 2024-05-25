# Ansible
## why Ansible 
 Ansible is valued for its simplicity, flexibility, scalability, and extensive capabilities, making it a preferred choice for automating infrastructure and application management tasks in diverse environments
### Installation
1 . At first broadcast up et disable ipv6

            For Debind l'ipv6 3 solutions : 
               1. command NMTUI then modifiy la connection , disable ipv6 et active connect automatic and then activate the connection
               2. Modify the file /etc/sysctl.conf and add the following lines: net.ipv6.conf.all.disable_ipv6 = 1 net.ipv6.conf.default.disable_ipv6 = 1 Then save and   
             restart with the sysctl -p command
               3.Otherwise modify the file /etc/default/grub add the line GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX ipv6.disable=1" Save and exit then restart again 
             grub file with the command grub2-mkconfig -o /boot/grub2/grub.cfg
                  Check dhcp ip address: ip a
2. Change default PC name by command line hostnamectl set-hostname XXX
3. Add user and his password then usermod group: add user

             adduser XXX
             passwd XXXX
             usermod -aG wheel XXX
             Check user in group wheel: cat /etc/group
             Check sudo:sudo vi /etc/passwd
4. Create ssh key on the server and send to clients (becareful to send which user)

               Ssh-keygen -t rsa -b 4096
               Ssh-copy-id user@ipadresse
6. Ansible installation (attention user, as user isadmin for example)

               1. Installing the repo requires epel-release: is a repository that provides additional packages for distributions based on RedHat
                     Cd /etc/yum.repos.d
                     Sudo yum install epel-release -y
            Note: for ansible classic need to do epel-release but for ansible core redhat already in repo
               2. Installation of ansible: Sudo yum install ansible -y
7.Edit the ansible host: you must put the IP address of all the clients in a group

             Exemple: sudo vi /etc/ansible/hosts
             [nom_group]
             Addresip1
             Addressip2  etc.
7.Collect of all informations on hosts

             Ansible -m setup nom-group
            The ansible -m setup host command is used to collect detailed information (called "facts") about the specified host (host)
8.Test all configuration ok by ping

                ansible -m ping nom_group
9.Create playbook and launch playbook

            An Ansible playbook is a file written in YAML (Yet Another Markup Language) that defines a series of instructions to be executed on one or more hosts (target machines). Playbooks are the primary way users define automation and configuration management tasks in Ansible.
            Create playbook: sudo vim createDirectory.yml
            Execute playbook: ansible-playbook nom_playbook -vK (v for message error, K for become:Utiliser become: true allows you to temporarily elevate the privileges of the current user for these specific tasks.)
            
            Example of writing a yaml file to create a directory:
![playbook-ansible](/Images/ans.jpg)

##Some important definistion:
- SUDO:This means that user can run any command as any user or group, on any machine (useful for multi-user or networked systems).
- SU: The su command in Unix and Linux systems stands for "substitute user" or "switch user." It allows a user to start a shell session as another user, typically the superuser (root).
- Yaml is a description language like xml, json etc.
- Handlesr: Handlers are a powerful mechanism in Ansible for handling follow-up actions after system state changes
- /etc/hosts (contains domain name information corresponding ip address)


## Roles in Ansible
- Ansible Roles provide a well-defined framework and structure for setting your tasks, variables, handlers, metadata, templates, and other files. They enable us to reuse and share our Ansible code efficiently.
## Why Roles Are Useful in Ansible
- Roles in Ansible provide a powerful way to organize, reuse, and manage your automation code. They enhance modularity, scalability, consistency, and collaboration, making it easier to handle complex configurations and infrastructure. By using roles, you can improve the maintainability, readability, and efficiency of your Ansible projects.

### Ansible Role Structure
       
       -> ansible-roles git:(master) ansible-galaxy init test_role
       - Role test_role was created successfully
  ![role-ansible](/Images/ans-role.jpg)  
  - defaults –  Includes default values for variables of the role. Here we define some sane default variables, but they have the lowest priority and are usually overridden by other methods to customize the role.
  - files  – Contains static and custom files that the role uses to perform various tasks.
  - handlers – A set of handlers that are triggered by tasks of the role. 
  - meta – Includes metadata information for the role, its dependencies, the author, license, available platform, etc.
  - tasks – A list of tasks to be executed by the role. This part could be considered similar to the task section of a playbook.
  - templates – Contains Jinja2 template files used by tasks of the role. (Read more about how to create an Ansible template.)
  - tests – Includes configuration files related to role testing.
  - vars – Contains variables defined for the role. These have quite a high precedence in Ansible.