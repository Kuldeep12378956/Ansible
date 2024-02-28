# Ansible

## Prerequesties - 
### 1. Atleast 2 Server (Ec2 Instance)
   Control Node- Need to install Ansible in it 
       `this` apt install absible -y.
       
   Managed Node - Need to install Python in it {# apt install python3 -y}

### 2. Enable password less Authentication between them. 
   Genrate SSH keys in both (Control Node & Managed Node)= 
            (#ssh-keygen)
   Save the public key of Contriol node into Managed Node Autharization_keys file.    


> [!NOTE]
> Make sure to give appropriate permission to public file
_________________________________________________________
 

### Adhoc Commands- 
Adhoc Command are used when there are minor Work Need to be done and writing playbook is not good thing to do it. 


