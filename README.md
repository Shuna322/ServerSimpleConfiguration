
# What this project about?
This repository contains Ansible playbook with tasks to automate my day to day work. A lot of times I need to deploy small project somewhere on the web on some cloud provider VM, usually this task requires to setup separate user, improve some security settings, install necessary packages, prepare docker, portainer, traefik, etc.

So now I no longer waste time for doing this work manually and simple host configuration with one command can do all the things for me.

In the end I receive server with oh-my-zsh, my custom dot files, docker, and disabled root authorization over ssh, so be careful :)

- [What this project about?](#what-this-project-about)
  - [How to use](#how-to-use)
    - [Requirements](#requirements)
    - [Quick local test](#quick-local-test)
    - [How to use in production](#how-to-use-in-production)
  - [End result](#end-result)

## How to use

### Requirements
1. Installed ansible on your local machine
2. **id_rsa** and **id_rsa.pub** in your **~/.ssh**
3. You can connect to your server with root@server_ip
4. Vagrant and VirtualBox is installed on your local machine ( **Only for local test** )
5. Review [vars.yml](group_vars/all/vars.yml) file. You can change list of pre installed packages, and also change username.
6. Replace secret file with password for future user. Use command:

```console
foo@bar:~$ rm ./group_vars/all/secret.yml && ansible-vault create ./group_vars/all/secret.yml
```

and create variable like this:

```yml
user_password: "Som3Pa33w0rd!"
```

### Quick local test
To quickly test end result you can use Vagrant to set up local VM and run playbook on it.

1. Boot up the machine with command:
   
```console
foo@bar:~$ vagrant up
```
2. Run playbook with staging inventory file:

```console
foo@bar:~$ ansible-playbook -i staging main.yml --ask-vault-pass -K
```

3. Connect to test user. ( **www** is the default )

### How to use in production

1. Prepare inventory file similar to [staging](staging).
2. Run main playbook, and provide passwords for secret file and become password. Example:

```console
foo@bar:~$ ansible-playbook -i production main.yml --ask-vault-pass -K
```

3. Verify that you can connect to host with username and ssh key that was used. ( **www** is the default )

## End result