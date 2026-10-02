# Create Files on App Servers using Ansible

The Nautilus DevOps team is testing various Ansible modules on servers in Stratos DC. They're currently focusing on file creation on remote hosts using Ansible. Here are the details:


a. Create an inventory file ~/playbook/inventory on jump host and include all app servers.


b. Create a playbook ~/playbook/playbook.yml to create a blank file /usr/src/webdata.txt on all app servers.


c. Set the permissions of the /usr/src/webdata.txt file to 0655.


d. Ensure the user/group owner of the /usr/src/webdata.txt file is tony on app server 1, steve on app server 2 and banner on app server 3.


Note: Validation will execute the playbook using the command ansible-playbook -i inventory playbook.yml, so ensure the playbook functions correctly without any additional arguments.

---

# Solution

- cd into `~/playbook` directory and create an `inventory` file:

```
[app_servers]
stapp01 ansible_user=tony ansible_password=Ir0nM@n ansible_connection=ssh
stapp02 ansible_user=steve ansible_password=Am3ric@ ansible_connection=ssh
stapp03 ansible_user=banner ansible_password=BigGr33n ansible_connection=ssh
```


- Create the playbook file:

```yaml
# playbook.yml

- hosts: all 
  become: yes 
  tasks: 
  - name: Create empty file 
    file: 
      path: /usr/src/webdata.txt 
      state: touch 
      owner: "{{ ansible_user }}" 
      group: "{{ ansible_user }}" 
      mode: "0655"
```


- Run the playbook:

`ansible-playbook -i inventory playbook.yml`

- Verify that the file is created on all app servers with correct user/group and permisions

- Run the playbook:
`ansible-playbook playbook.yml -i inventory`

