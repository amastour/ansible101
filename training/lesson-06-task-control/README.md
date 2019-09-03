# Ansible Conditionals

Conditionals are one of the fundamental parts of any programming languages so as to control the flow of execution. Ansible also provides a method for this using the `when` clause. The basic `when` operation is very easy. You just have to declare the condition against the when clause like below.
```yaml
---
- hosts: apache
  become: yes
  tasks:
   - name: Install the Nginx HTTP Server on Debian dist
          apt: 
                name: nginx
                update_cache: yes 
                state: present             
          when: ansible_os_family == "Debian"
```
Now run this playbook:
```shell
vagrant@Control:~/ansible/training$ ansible-playbook -i inventory/hosts lesson-06-task-control/lesson-06.yml

PLAY [web] *************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [loadbalancing]

TASK [Install the Nginx HTTP Server on Debian dist] ********************************************************************
changed: [loadbalancing]

TASK [Install the Nginx HTTP Server on Debian dist] ********************************************************************
skipping: [loadbalancing]

TASK [create html directory] *******************************************************************************************
changed: [loadbalancing]

TASK [copy index.html] *************************************************************************************************
changed: [loadbalancing]

PLAY RECAP *************************************************************************************************************
loadbalancing              : ok=4    changed=3    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

vagrant@Control:~/ansible/training$
```
If you want to read more about template, please follow this [link](https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html)

