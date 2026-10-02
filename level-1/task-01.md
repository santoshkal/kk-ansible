# Troubleshoot and Create Ansible Playbook

An Ansible playbook needs completion on the jump host, where a team member left off. Below are the details:



- The inventory file /home/thor/ansible/inventory requires adjustments. The playbook must run on App Server 1 in Stratos DC. Update the inventory accordingly.


- Create a playbook /home/thor/ansible/playbook.yml. Include a task to create an empty file /tmp/file.txt on App Server 1.


Note: Validation will run the playbook using the command ansible-playbook -i inventory playbook.yml. Ensure the playbook works without any additional arguments.

--- 

# Solution

- cd `/root/thor/ansible` and open `inventory` file. The inventory file contains config for `stapp03`, but task need to be for `stapp01`:

`stapp03 ansible_user=banner ansible_ssh_pass=$pwd ansible_ssh_common_args='-o StrictHostKeyChecking=no'`

- Update the `ansible_host`, `ansible_user`, and correct `ansible_ssh_pass` for the host desired in the task:

`stapp01 ansible_host=stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'`

- Create a `playbook.yml` for second requirement:

```yaml
---
- name: Create an empty file on App Server 1
  hosts: stapp01
  become: true
  tasks:
    - name: Create empty file
      ansible.builtin.file:
        path: /tmp/file.txt
        state: touch
```

- Run the playbook:

`ansible-playbook ./playbook.yml -i inventory`
