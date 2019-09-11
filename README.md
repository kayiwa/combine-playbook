# Combine Playbook
This repository contains code that will deploy the ecosystem necessary to run [Combine](https://github.com/WSULib/combine). There are two deployment methods explained below. Choose the one that meets your needs.

Please be aware that running this system requires not insignificant resources. Required is at least 12-16gb RAM and 2+ CPU cores for the fully built server.  This is due in large part to the memory requirements of Apache Spark.

If you are looking to test-drive, install, or develop on [Combine](https://github.com/WSULib/combine), you have arrived at the right place!

## QuickStart walkthrough
Once you do have an instance of the server up and running, you can find a [QuickStart walkthrough here](https://github.com/WSULib/combine/blob/dev/docs/quickstart.md).

## Install Tagged Releases:
For releases, there will be an attempt to pin releases from this [Combine Playbook](https://github.com/WSULib/combine-playbook) repository to the [Combine](https://github.com/WSULib/combine) repository with the same tag.  This is set in `combine_git_branch` setting under `group_vars/all.yml`.  

To build a release, before any steps below, checkout the [release/tag](https://github.com/WSULib/combine-playbook/releases) of this playbook you are interested in building and note the release number like `v0.3.1` or `v0.4`.  For example, to build version `v0.3.1`, do the following:

```
# clone playbook repo
git clone https://github.com/WSULib/combine-playbook
cd combine-playbook

# checkout v0.3.1 tag
git checkout v0.3.1 
```

This will then attempt to build the `v0.3.1` tagged release of [Combine](https://github.com/WSULib/combine).

## Pre-Installation Notes:
 * This deployment assumes an Ubuntu 18.04 server
 * For either installation, there are a host of variables that set default values. They are all found in the `all.yml` file inside the `group_vars` folder.
   * If you are installing this system on a remote server, you MUST update the `ip_address` variable found in `all.yml`. Change it to your remote server's ip address.
   * If you are installing the system locally with Vagrant, you don't need to do anything. Your server will be available at 192.168.45.10.


## Vagrant-based Installation (local)

 * If you are looking to run an instance of the Combine ecosystem on your own computer, you will use the Vagrant-based installation method. This method assumes that you have 4GB of RAM and 2 processor cores available to devote to this system. Double-check and make sure you have this available on your computer. This means you will need MORE than that in RAM and cores in order to not bring your computer to a complete halt. Default settings create a VM that uses 4GB of RAM and 2 cores. Local testing has been performed on iMacs running MacOS Sierra that have a total of 4 cores and 16 GB of RAM.

 * Install [VirtualBox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com/).

 * Clone this Github repository.

 * Navigate to the repository in your favorite terminal/shell/command line interface.

 * Build the system.
   ```
   vagrant up
   ```
 * This installation will take a while. The command you just ran initializes the vagrant tool to manage the installation process. It will first download and install a copy of Ubuntu Linux (v.18.04) on your VirtualBox VM. Then, it will configure your networking to allow SSH access through an account called `vagrant` and make the server available only to your local computer at the IP address of 192.168.45.10. After that initial work, the vagrant tool will use ansible to provision (i.e. install all components and dependencies) to a VM on your computer.

 * After completed, your server will be available at [http://192.168.45.10](http://192.168.45.10). Navigating to [http://192.168.45.10/admin](http://192.168.45.10/admin) will allow you to setup your system defaults (OAI endpoints, etc). Going to [http://192.168.45.10/combine](http://192.168.45.10/combine) will take you to the heart of the application where you can ingest, transform, and analyze metadata. Login using the credentials the following credentials: 
   ```
   username: combine
   password: combine
   ```

 * Access via SSH is available through the accounts below. Both have sudo privileges. The combine password defaults to what is listed below. If you have edited `group_vars/all.yml` and changed the password listed there, please adjust accordingly.
    ```
    username: combine
    password: combine

    username: vagrant
    password: vagrant
    ```

## Ansible-based Installation (remote server)

 * If you have a remote server that you want to install the system upon, these installation instructions are for you. Your server should already be running Ubuntu 18.04. It needs to be remotely accessible through SSH from your client machine and have at least port 80 accessible. Also, it needs Python 2.7 installed on it. Your server will need at least 8GB of RAM and 2 cores, but more is better.

 * Install [Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html), [Python](https://www.python.org/), and [Passlib](https://pypi.python.org/pypi/passlib) on your client machine. This installation method has not been tested using Windows as client machine, and, therefore, we offer no support for running an installation using Windows as a client. For more information, please refer to these Windows-based instructions: http://docs.ansible.com/ansible/latest/intro_windows.html#using-a-windows-control-machine
   * NB: when installing Passlib, you should be able to simply run `pip install passlib` if you have the pip tool installed. If you're not certain or if that command doesn't successfully run, see the following link for instructions on installing Pip: http://www.pythonforbeginners.com/basics/how-to-use-pip-and-pypi.

 * Exchange ssh keys with your server. 
   * Example command on MacOS

     ```
     ssh-keygen -t rsa
     cat ~/.ssh/id_rsa.pub | ssh USERNAME@IP_ADDRESS_OR_FQDN "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"
     ```

 * Point ansible to remote server. 
   * You do this by creating a file named `hosts` inside the following directory: `/etc/ansible`. If you are using a Linux or MacOS machine, you should have an `etc` directory, but you will probably have to create the `ansible` folder. Place your server's IP address or FQDN in this `hosts` file. If the username you used to exchange keys with the server is anything other than root, you will have to add `ansible_user=YOUR_USERNAME`. Your hosts file could end up looking something like this: `192.168.45.10 ansible_user=USERNAME`. For more information see: http://docs.ansible.com/ansible/latest/intro_getting_started.html#your-first-commands

 * Check your target machine is accessible and ansible is configured by running the following command:
   ```
   ansible all -m ping
   ```
   * A successful response will look something similar to this. Substitute your IP for the one listed below in the example.
     ```
     192.168.44.10 | SUCCESS => {
     "changed": false, 
     "failed": false, 
     "ping": "pong"
     }
     ```

 * Clone this Github repository

 * Update `ip_address` in `group_vars/all.yml`
   * Change the `ip_address` variable to your remote server's IP address.

 * Install pre-requisites (NOTE: You may need to make sure the roles are being installed in the repository's `roles` directory with `... -p ./roles ...`)
   ```
   ansible-galaxy install -f -c -r requirements.yml
   ```

 * Run ansible playbook
   ```
   ansible-playbook -vvvvv playbook.yml
   ```

 * This installation will take a while. Ansible provisions the server with all of the necessary components and dependencies.

 * After the installation is complete, your server will be ready for you to use Combine's web-based interface. Go to your server's IP address. Navigating to `/admin` will allow you to setup your system defaults (OAI endpoints, etc). Going to `/combine` will take you to the heart of the application where you can ingest, transform, and analyze metadata. Login using the following credentials: 
   ```
   username: combine
   password: combine
   ```

 * Access via SSH is available through the account below. It has sudo privileges. The password below is correct unless you have changed it inside `group_vars/all.yml`.
   ```
   username: combine
   password: combine
   ```
   
## Troubleshooting

### Livy/Spark not working

If the Livy logs include errors like this:
```
19/08/19 11:43:30 ERROR rsc.RSCClient: Failed to connect to context.
```
check for an error like this:
```
19/08/19 11:21:25 INFO utils.LineBufferedStream: 2019-08-19 11:21:25 WARN  RSCConf:146 - Your hostname, [blahblahblah], resolves to a loopback address, but we couldn't find any external IP address!
```
If you find that, add the hostname to `/etc/hosts`:
```
127.0.0.1 localhost blahblahblah blahblah_ip
```

### Restarting Elasticsearch
```
sudo systemctl restart elasticsearch.service
```

### Transforms failing

If your XSLT transforms are failing with an error like `NoneType has no such method 'transform'`, double-check that any includes inside your transform XSLT use correct locations. This is particularly important to check if you have imported any transformations that used local includes.
