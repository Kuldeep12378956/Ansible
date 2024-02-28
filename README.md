# Ansible

## Prerequesties - 
### 1. Atleast 2 Server (Ec2 Instance)
Control Node- Need to install Ansible in it
    `apt install absible -y'
Managed Node - Need to install Python in it 
    'apt install python3 -y'

### 2. Enable password less Authentication between them. 
   Genrate SSH keys in both (Control Node & Managed Node)= 
            (#ssh-keygen)
   Save the public key of Contriol node into Managed Node Autharization_keys file.    


> [!NOTE]
> Make sure to give appropriate permission to public file
_________________________________________________________
# Modules -  

There are multiple modules, Ansible keep creating new Modules. Few of the very famous Modules are. 
1. File -
   States of File Module = {directory, tocuh, absent, etc }
2. Shell Module {using this module we can run any Shell command on managed Node.}
3. Ping Module {user to ping the group of the Servers}
4. Package Module - {to install, update and remove package.}
5. Service Module - {To restart, stop, status check the service}
6. Copy Module -
7. user Module- 



# Adhoc Commands- using Various Modules
Adhoc Command are used when there are minor Work Need to be done and writing playbook is not good thing to do it. 

# Ping Module


### 1 ansible webservers -m ping

This will ping the Group of Webservers in host file. 
___________________________________________________ 

# file Module

### 1 ansible webservers -m file -a 'dest=/home/ubuntu/test_dir mode=0755 owner=ubuntu group=ubuntu state=directory'

This will Create the text_dir at group of Servers named Webservers, and give them specific permission

-m = module
file = module type 
-a = arigument

### 2 ansible webservers -m file -a 'dest=/home/ubuntu/file1 mode=0755 owner=ubuntu group=ubuntu state=touch'

this will create a new file at remote serer with mentioned premission.

### 3 ansible webservers -m file -a 'dest=/home/ubuntu/test_dir  state=absent'

This command will delete the test_dir from all the servers under webservers group. 

*************************************************************************************************************

# Package Module- 

### 1 ansible webservers -m apt -a 'name=python3 state=present update_cache=yes' -b
This will install the python package in the Webserver Group.

-m = module
-a = argument
name= package name 
*update_cache= yes* is directory equal to -y in  *apt-install python -y* 
-b = this flag tell command to be become root user to install package. 

### ansible webservers -m apt -a 'name=python3 state=absent' -b

This command will uninstall Python package from webservers group.

### ansible webservers -m apt -a 'name=python3 state=latest' -b

This command will update the package to latest.


****************************************************************************************************************

# Service Module - This module will help to Manupulate the Service. 

### ansible webservers -m service -a 'name=cron enabled=no' -b| more

This command will disable the service named cron 

### ansible webservers -m service -a 'name=cron enabled=yes' -b| more

This command will enable the cron service 

### ansible webservers -m service -a 'name=cron state=stopped' -b

This command will stop the service Cron in the remote server.

### ansible webservers -m service -a 'name=cron state=started' -b

This commnd will start the Cron service in the remote server.

******************************************************************

# Copy Module - This Module will helo with copy file from the control Node to the managed Node. 

### ansible webservers -m copy -a 'src=/home/abc.txt dest=/home/abc.txt owner=ubuntu group=ubuntu mode=0640' -b

This command will copy the the abc.txt file from source location to destination location.

*******************************************************************

# User Module - Will help to create, remove and create SSH key for user. 

### ansible webservers -m user -a 'name=kuldeep shell=/bin/bash' -b

This command will Create the user and give it shell access.

###  ansible webservers -m user -a 'name=kuldeep state=absent remove=yes' -b

This command will remove the user

**********************************************************


# Ansible Playbook- The playbook 

Play book can help us to automate multiple task and the main benifit of it is that it doesn't repeat the tasks as like Adhoc commands, 

Below playbook will install apache service, enable it, remove the previos index.html file, create the new one, and add the content inside the file. 

## These are the few important command to be remembered. 
1. Create a Directory to store the playbook. any where in file system.
2. create the playbook with yml format
3. once done. test that playbook for syntax error with dryrun option {ansible-playbook apache-install.yml -C}
4. Finally run the playbook. by command {ansible-playbook apache-install.yml }



### Ansible Playbook Example - 

---
- name: install and configure Apache
  hosts: webservers
  become: yes
  become_user: root

  tasks:
   - name: install Apache
     apt:
       name: apache2
       state: latest
       update_cache: yes
   - name: Enable service and start service
     service:
       name: apache2
       enabled: yes
       state: started
   - name: remove the existing index.html
     file:
        path: /var/www/html/index.html
        state: absent
   - name: Create new index.html path
     file:
        path: /var/www/html/index.html
        state: touch
        owner: root
        group: root
        mode: 0755
   - name: Add content in index.html file
     lineinfile:
        path: /var/www/html/index.html
        line: Hello Anisble World!

        












