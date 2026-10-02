# Copy Data to App Servers using Ansible

The Nautilus DevOps team needs to copy data from the jump host to all application servers in Stratos DC using Ansible. Execute the task with the following details:


a. Create an inventory file /home/thor/ansible/inventory on jump_host and add all application servers as managed nodes.


b. Create a playbook /home/thor/ansible/playbook.yml on the jump host to copy the /usr/src/finance/index.html file to all application servers, placing it at /opt/finance.


Note: Validation will run the playbook using the command ansible-playbook -i inventory playbook.yml. Ensure the playbook functions properly without any extra arguments.

---

# Solution:

- cd into `/home/thor/ansible` and create an `inventory` file and add all the appservers:

```
[app_servers]
stapp01 ansible_user=tony ansible_password=Ir0nM@n ansible_connection=ssh
stapp02 ansible_user=steve ansible_password=Am3ric@ ansible_connection=ssh
stapp03 ansible_user=banner ansible_password=BigGr33n ansible_connection=ssh
```


- Create the playbook file:

```yaml
# playbook.yml

- name: Copy files
  hosts: app_servers
  become: yes
  tasks:
    - name: Copy file
      copy:
        src: /usr/src/finance/index.html
        dest: /opt/finance/
```


- Run the playbook:
`ansible-playbook playbook.yml -i inventory`

Verify on all app servers that the `/opt/finance/index.html` exists.
