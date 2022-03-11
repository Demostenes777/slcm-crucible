# Installing crucible in a fully virtualized environment

This document gives the instructions to install an OpenShift cluster in a fully virtualized environment using crucible.

## Prerequisites

The host is running RHEL 8.5 and has a registered subscription

### Login as redhat user

If the user does not exist, create it

> $ sudo useradd redhat
> $ su redhat

### Check VM network configuration

You will need two network interfaces. The first one (enp1s0 in the diagram) must have access to the Internet. The second one (enp7s0 in
the diagram) does not need an IP address, just needs to be up.

### Install Ansible

Install the EPEL repository

> $ sudo subscription-manager repos --enable "codeready-builder-for-rhel-8-$(arch)-rpms"
>
> $ sudo yum -y install <https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm>

Install Ansible

> $ sudo yum install ansible

### Download slcm-crucible automation playbooks

On your VM, download the slcm-crucible project in your home

> $ cd ~
> 
> $ git@gitlab.cee.redhat.com:red-hat-juniper-joint-lab/slcm-automation/slcm-crucible.git

### Edit secret.yml

Edit the file ~/slcm-crucible/vars/secret.yml and ensure that the three variables have been updated with your information

> $ vim ~/slcm-crucible/vars/secret.yml
> 
> REDHAT_SUBSCRIPTION_USERNAME: add_your_username_here
> 
> REDHAT_SUBSCRIPTION_PASSWORD: add_your password_here

### Encrypt secret.yml with ansible-vault

> $ ansible-vault encrypt ~/slcm-crucible/vars/secret.yml

## Prepare the host

Execute pre-deployment.yml playbook to install all the required dependencies and run scenario-specific steps.

> $ ansible-playbook ~/slcm-crucible/pre-deployment.yml --ask-vault-pass

You can see the tasks performed by the pre-deployment.yml in [deploying_all_in_one.md](deploying_all_in_one.md)