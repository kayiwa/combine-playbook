# combine-playbook
Ansible playbook for deploying combine application to remote server.


 * Works on Ubuntu 16.04 server

### Installation Steps

 * Install ansible [http://docs.ansible.com/ansible/latest/intro_installation.html] on your client machine. If you're using Windows as your client, please refer to these instructions: http://docs.ansible.com/ansible/latest/intro_windows.html#using-a-windows-control-machine

 * Exchange ssh keys with your server. 

 * Example command on MacOS

   ```
   ssh-keygen -t rsa
   cat ~/.ssh/id_rsa.pub | ssh USERNAME@IP_ADDRESS_OR_FQDN "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"
   ```

 * Point ansible to remote server
   Create /etc/ansible/hosts and place the IP address or FQDN in this file. See: http://docs.ansible.com/ansible/latest/intro_getting_started.html#your-first-commands

 * Clone the Github repository

 * Install requirements
   ```
   ansible-galaxy install -r requirements.yml
   ```

 * Run ansible playbook
   ```
   ansible-playbook playbook.yml
   ```
