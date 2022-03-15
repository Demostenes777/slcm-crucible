# Installing crucible in a fully virtualized environment

This document gives the instructions to install an OpenShift cluster in a fully virtualized environment using crucible.

## Prerequisites

The host is running RHEL 8.5 and has a registered subscription

### Login as redhat user

If the user does not exist, create it, be sure to grant the user admin rights

> # useradd redhat
> # usermod -aG sudo redhat
> # su redhat

### Create the crucible group

> $ sudo groupadd crucible

Add the redhat user to the crucible group

> $ sudo usermod -aG crucible redhat

### Check VM network configuration

You will need two network interfaces. The first one (enp1s0 in the diagram) must have access to the Internet. The second one (enp7s0 in
the diagram) does not need an IP address, just needs to be up.

### Create ssh key

> $ ssh-keygen -t rsa -N '' -b 4096
> $ ssh-copy-id redhat@localhost

### Install Ansible

Install the EPEL repository

> $ sudo subscription-manager repos --enable "codeready-builder-for-rhel-8-$(arch)-rpms"
>
> $ sudo yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

Install Ansible

> $ sudo yum install ansible

### Install git package

> $ sudo yum install git

### Download the pull secret

Please [download](https://console.redhat.com/openshift/install/metal/installer-provisioned)
the pull secret and place it at your home directory

### Download slcm-crucible automation playbooks

On your VM, download the slcm-crucible project in your home

> $ cd ~
> 
> $ git clone https://github.com/Demostenes777/slcm-crucible

### Edit secret.yml

Edit the file ~/slcm-crucible/vars/secret.yml and ensure that the three variables have been updated with your information

> $ vim ~/slcm-crucible/automation/vars/secret.yml
> 
> REDHAT_SUBSCRIPTION_USERNAME: add_your_username_here
> 
> REDHAT_SUBSCRIPTION_PASSWORD: add_your password_here

### Encrypt secret.yml with ansible-vault

> $ ansible-vault encrypt ~/slcm-crucible/vars/secret.yml

### Edit inventory.vault.yml

Edit the file ~/slcm-crucible/inventory.vault.yml and add your sensitive information

> $ vim ~/slcm-crucible/inventory.vault.yml

### Encrypt inventory.vault.yml with ansible-vault

> $ ansible-vault encrypt ~/slcm-crucible/inventory.vault.yml

## Prepare the host

Execute pre-deployment.yml playbook to install all the required dependencies and run scenario-specific steps.

> $ ansible-playbook ~/slcm-crucible/pre-deployment.yml --ask-vault-pass

You can see the tasks performed by the pre-deployment.yml in [deploying_all_in_one.md](deploying_all_in_one.md)