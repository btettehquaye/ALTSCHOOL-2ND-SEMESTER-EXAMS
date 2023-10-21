# DEPLOYMENT OF PHP LARAVEL ON UBUNTU LAMP STACK 

![Alt text](<LARAVEL SLAVE 1.png>)



This documentation is a very simple explanation of how to use the repo above to run the laravel application on an Ubuntu machine with a LAMP (Linux, Apache, MySql and php) stack  present.

Apache is going to be used as the web-sever while MySql will be used as the data base and php will be the base language laravel runs on.

there is also a master and slave machine that will be created using a bash-script. on the master we are going to be running a script that installs LAMP Stack and deploy laravel 

> **heads up**: *the script is non-interactive*

and on the slave we are going to be using an ansible playbook to run the same script that deploys laravel and LAMP Stack with a little difference 

Ansible will also be helping us create a cron job the checks the servers uptime every 12 AM

ready? let's get started! 

---

## BREAK DOWN OF THE REPO

looking at the repo above you can tell that there are just 2 directorties present. which are:

- Ansible-playbook
- bash-script

we will be looking at each one to get a deeper understanding of what the two directories do.

---
### Ansible-Playbook

going into the ansible playbook you would discover there are 3 files present:

- ansible.cfg
- bash.yml
- inventory

the ansible.cfg file holds all our configuration for ansible.

the bash.yaml holds all the tasks and plays you are going to run

the inventory file holds the  domain or ip address you want to deploy the Laravel application to.

there is also a directory named **roles** and this is where the ansible roles will be placed 

> there are just 2 ansible roles which are:

- copy_file
- server_uptime

the **copy_file** role copies the bash script that deploys laravel, make it executable and run it using the **commands** ansible module.

the **server_uptime** role creates a cron job that checks the servers uptime every 12 AM using ansible.


---

### Bash-Script

the bash script directory contains just 2 files:

- master-slave.sh
- bash-script.sh

the **master-slave** file hold the script that deploys a master and slave ubuntu vagrant machine with a static ip address.

> slave: 192.168.20.11

> master: 192.168.20.10

the **bash-script** file holds the script to deploy LAMP Stack and the laravel application on the master and slave machine

> **Note:** *the script is non-interactive*

# HOW TO RUN THE SCRIPT

this script is very readable and reuseable because of that it is flexible. 

the bash-script is to run when the master machine is up

to run this script you will need to consider just 4 things

1. MySql database
2. .ENV (laravel file)
3. Ansible.config file

## MySql

due to the fact that we are using parameters to create a database while running the script you will need just 2 arguments:

```bash
./bash-script teddi teddi90
```
the 1st ARG is for the DB_USERNAME and DB_DATABASE

the 2nd ARG is for the DB_PASSWORD

> **NOTE**: whatever username and password you are using it must align with the username and password in the .env file

## .ENV (laravel file)

since everything is running automatically we will also need to change the .env file automatically:

```bash
sudo sed -i 's/DB_DATABASE=laravel/DB_DATABASE=teddi/' /var/www/html/laravel/.env

sudo sed -i 's/DB_DATABASE=laravel/DB_DATABASE=teddi/' /var/www/html/laravel/.env

sudo sed -i 's/DB_PASSWORD=/DB_PASSWORD=teddi90/' /var/www/html/laravel/.env
```

make sure this three lines correspond with the ARG you will be adding while running the script.

you can find this code in the **bash-script** file and make changes to only the results after the = symbol on the right hand side.

## Ansible.config file

in the **bash-script** file there is a section for the ansible config file.

in this path all you will need to change is the **ServerName** and you will be changing this to the domain name or ip address of the server you want to run the script on.

---
if you have all this set then you are ready to run the script like i showed you how.

