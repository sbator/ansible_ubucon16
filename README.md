# Setting up the environment

This repository contains everything you need to replicate the examples of my talk.
This code sets up three LXD containers that serve as hosts. Using LXD enables 
the Ansible playbooks to run as local user, furthermore the playbooks do not 
change anything on the host (except creating the containers and changing files 
that are part of this repository). To run them however, LXD and Ansible >2.2 are
needed. The setup instructions can be found below, these commands install new 
packages and therefore change the host computer.

## Setting up Ansible and LXD

The LXD module needs at least Ansible version 2.2. At the moment this means we
need to install it via the Ansible PPA.
```
sudo add-apt-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

Furthermore LXD is required and you need to configure an LXD network for your
containers.
```
sudo apt install LXD
sudo dpkg-reconfigure -p medium LXD
# Answer the questions and create an NATed IPv4 network for LXD
# If you are using a Firewall, it might need further configuration
# to allow traffic from the containers.
```

## Run the playbooks

`setup-env.yml` creates the containers and provisions them with a basic webserver.
`my_config.yml` deploys some example configuration files the containers them. 
You need to run Ansible in the root of the git checkout.

```
ansible-playbook -i inventory setup-env.yml
ansible-playbook -i inventory my-config.yml
```

## Clean up

The LXD containers will keep running in the background. If you no longer need
them, you can stop or delete them.
```
lxc stop ubu{1..3}
lxc destroy ubu{1..3}
```


Have fun

Sebastian
