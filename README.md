# Ansible
## why Ansible 
 Ansible is valued for its simplicity, flexibility, scalability, and extensive capabilities, making it a preferred choice for automating infrastructure and application management tasks in diverse environments
### Installation
1 . At first broadcast up et disable ipv6

   For Debind l'ipv6 3 solutions : 
      1. command NMTUI then modifiy la connection , disable ipv6 et active connect automatic and then activate the connection
      2. Modify the file /etc/sysctl.conf et ajouter les lignes suivantes : net.ipv6.conf.all.disable_ipv6 = 1 net.ipv6.conf.default.disable_ipv6 = 1  Puis sauvegarder et   
   relancer avec la commande sysctl -p
      3. Sinon modifier le fichier /etc/default/grub Y ajouter la ligne GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX ipv6.disable=1" Sauvegardez et quittez puis relancez un nouveau 
   fichier grub avec la commande grub2-mkconfig -o /boot/grub2/grub.cfg
        Check dhcp ip addresse : ip a
2. Changer nom pardefaut de pc  by ligne de commande hostnamectl set-hostname XXX
3. Add user and his password then user mod group: add user
      adduser XXX
      passwd XXXX
      usermod -aG wheel XXX
      Verifer user dans le group wheel: cat /etc/group
      Verifier sudo :sudo vi /etc/passwd
4. Creer clé ssh au serveur et enovyer aux clients (attention user)
      Ssh-keygen -t rsa -b 4096
      Ssh-copy-id user@ipadresse
5. Installation ansilbe (attention user , en tant que user osadmin pour exemple)
      1.Installation du repo il faut faire epel-release: est un dépôt qui fournit des paquets additionnels pour les distributions basées sur RedHat
      Cd /etc/yum.repos.d
      Sudo yum install epel-release -y
      Note: pour ansible classic besoin de faire epel-release mais pour ansible core redhat deja dans  repo
      2.Installation de ansible: Sudo yum install ansible -y
6. Editer le host ansible : il faut mettre addresse ip de tous les client dans un group
      Exemple: sudo vi /etc/ansible/hosts
      [nom_group]
      Addresip1
      Addressip2  etc.
7.Collecter les information sur les host
      Ansible -m setup nom-group
      La commande ansible -m setup host est utilisée pour collecter des informations détaillées (appelées "faits") sur l'hôte spécifié (host)
8.Tester toutes configuration ok par ping
      ansible -m ping nom_group
9.Creer playbook et lancer playbook
      Un playbook Ansible est un fichier écrit en YAML (Yet Another Markup Language) qui définit une série d'instructions à exécuter sur un ou plusieurs hôtes (machines cibles). Les playbooks sont le moyen principal par lequel les utilisateurs définissent des tâches d'automatisation et de gestion de configuration dans Ansible.
      Creer playbook: sudo vim createDirectory.yml
      Execute playbook: ansible-playbook nom_playbook -vK (v for message error, K for become:Utiliser become: true permet d'élever temporairement les privilèges de l'utilisateur courant pour ces tâches spécifiques.)
      Exemple ecrire un fichier yaml pour creer un directory:

##Some important definistion:
- SUDO: Cela signifie que l'utilisateur user peut exécuter n'importe quelle commande en tant que n'importe quel utilisateur ou groupe, sur n'importe quelle machine (utile pour les systèmes multi-utilisateurs ou en réseau).
- SU: The su command in Unix and Linux systems stands for "substitute user" or "switch user." It allows a user to start a shell session as another user, typically the superuser (root).
- Yaml est un langauge de description comme xml, json etc
- Handlesr: Les handlers sont un mécanisme puissant dans Ansible pour gérer les actions de suivi après des changements d'état du système
- /etc/hosts (contiens des informations de nom de domaine corresponding ip address)
