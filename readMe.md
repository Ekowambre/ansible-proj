Hello Rockians, please let the diagram give you a pictorial framework of what the project should be.

1. Create an ec2 Server on the AWS console called Ansible machine (use amazon linux2 image)

2. Create 3 ec2 servers called Server1, server2, server3 (use amazon Linux 2 image)

3. Create SSH tunnels and establish a connection between master and the servers
(NB windows and mac users, use the right means to SSH)

4. Install ansible on the ansible machine with commands below:
sudo yum update -y
sudo amazon-linux-extras install ansible2 -y

5. create the inventory file and name it as inventory

6. create playbook file with the name CRA-playbook.yml and paste the below in the file:
---
- name: deploy techmax website
 hosts: all
 become: yes
 become_user: root
 tasks:
 - name: update ec2 instance
 yum:
 name: "*"
 state: latest
 update_cache: yes
 - name: install apache server
 yum:
 name: httpd
 state: latest
 - name: change directory to the html directory
 shell: cd /var/www/html
 - name: download web files from github
 get_url:
 url: https://github.com/azeezsalu/techmax/archive/refs/heads/main.zip
 dest: /var/www/html/
 - name: unzip the zip folder
 ansible.builtin.unarchive:
 src: /var/www/html/techmax-main.zip
 dest: /var/www/html
 remote_src: yes
 - name: copy webfiles from the techmax-main directory to the html directory
 copy: 
 src: /var/www/html/techmax-main/
 dest: /var/www/html
 remote_src: yes
 - name: remove the techmax-main directory
 file: 
 path: /var/www/html/techmax-main
 state: absent
 - name: remove the techmax-main.zip folder
 file: 
 path: /var/www/html/techmax-main.zip
 state: absent 
 - name: start apache server, if not started
 ansible.builtin.service:
 enabled: yes
 name: httpd
 state: started


7. ping to check all servers are successful with below command:
ansible all –key-file ~/.ssh/id_rsa -I inventory -m ping -u ec2-user

8. create config file with name ansible.cfg and put the below in the file
[defaults]
remote_user = ec2-user
inventory = inventory
Private_key_file = ~/.ssh/id

9. run the ansible playbook
 search the command for this
 
10. when successfully done, please copy and paste the public Ip of the any of the servers from 
the AWS console and launch on a new tab to see the results.
 Take a screen shot and send to the email add for assignments.# ansible-proj
