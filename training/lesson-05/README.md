# Ansible files

Ansible provides the basic functionality of copying files and directories through the copy and fetch modules.
You can use the copy module to copy files and folders from the local server to the remote servers, between remote servers(only files), change the permission of the files, etc.

By default, the copy module will check the file set in the `src` parameter, on the local machine. And then it will copy the file to the remote machine path specified in the `dest` path. The example below will copy the index.html file in the html directory of the files folder, to the /var/www/html directory on the remote server. Since we are not specifying any permission for the file, the default permission for the remote file is set as -rw-rw-r–(0664).

```yaml
---
- hosts: apache
  become: yes
  tasks:
    - name: create target directory
    file:
            path: /var/www/html
            state: directory
            mode: 0755
    - name: copy index.html
    copy:
            src: files/index.html
            dest: /var/www/html/index.html
```
Now run this playbook:
```shell
vagrant@Control:~/ansible$ ansible-playbook -i training/inventory/hosts training/lesson-05/lesson-05.yml

PLAY [apache] **********************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [web1]
ok: [web2]

TASK [create target directory] *****************************************************************************************
changed: [web1]
changed: [web2]

TASK [copy index.html] *************************************************************************************************
changed: [web1]
changed: [web2]

PLAY RECAP *************************************************************************************************************
web1                       : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
web2                       : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

vagrant@Control:~/ansible/$
```

