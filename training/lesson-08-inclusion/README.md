# Ansible Inclusion
It is possible to write a playbook in a very large file (and you could start by learning this way), you will eventually want to reuse files and start organizing things.
In Ansible, there are three ways to do it: inclusions, imports, and roles.

Inclusions and imports allow users to divide big playbooks into smaller files, which can be used on primary playbooks or even multiple times within the same playbook.
Roles allow you to group more than just tasks, and can include variables, handlers, and even modules. Unlike inclusions and imports, roles can also be uploaded and shared via Ansible Galaxy.


Let's take our previous playbook that we have used to install Nginx, this time, we'll splite into different files.
```yaml
---
- hosts: front
  become: yes
  tasks:
        - name: include vars for Debian dists
          include_vars: vars_debian.yml
          when: ansible_os_family == "Debian"
        
        - name: include vars for RedHat dists
          include_vars: vars_redhat.yml   
          when: ansible_os_family == "RedHat"

        - name: call Install  Nginx for Debian dists
          include_tasks: install_debian.yml
          when: ansible_os_family == "Debian"
        
        - name: call Install  Nginx for RedHat dists
          include_tasks: install_redhat.yml   
          when: ansible_os_family == "RedHat"
        
        - name: call configure  Nginx for Debian dists
          include_tasks: configure_debian.yml
          when: ansible_os_family == "Debian"
        
        - name: call configure  Nginx for RedHat dists
          include_tasks: configure_redhat.yml
          when: ansible_os_family == "RedHat"
        
        - debug:
                msg: "{{ success_message }}"
```
Now run this playbook:
```shell
vagrant@Control:~/ansible/training$ ansible-playbook -i inventory/hosts lesson-08-inclusion/lesson-08.yml

PLAY [front] *************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [loadbalancing]

TASK [include vars for Debian dists] ***********************************************************************************
ok: [loadbalancing]

TASK [include vars for RedHat dists] ***********************************************************************************
skipping: [loadbalancing]

TASK [call Install  Nginx for Debian dists] ****************************************************************************
included: /home/vagrant/ansible/training/lesson-08-inclusion/install_debian.yml for loadbalancing

TASK [Nginx - Install the package Debian] ******************************************************************************
 [WARNING]: Could not find aptitude. Using apt-get instead

changed: [loadbalancing]

TASK [call Install  Nginx for RedHat dists] ****************************************************************************
skipping: [loadbalancing]

TASK [call configure  Nginx for Debian dists] **************************************************************************
included: /home/vagrant/ansible/training/lesson-08-inclusion/configure_debian.yml for loadbalancing

TASK [ensure html directory exist] *************************************************************************************
changed: [loadbalancing]

TASK [add  index.html] *************************************************************************************************
changed: [loadbalancing]

TASK [call configure  Nginx for RedHat dists] **************************************************************************
skipping: [loadbalancing]

TASK [debug] ***********************************************************************************************************
ok: [loadbalancing] => {
    "msg": "nginx installed successfully"
}

PLAY RECAP *************************************************************************************************************
loadbalancing              : ok=8    changed=3    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0

vagrant@Control:~/ansible/training$
```
Let's verify that the index page is added:
```shell
vagrant@Control:~/ansible/training$ curl http://192.168.33.30
<!DOCTYPE html>
<html>
        <head>
                <title>deployed for Ubuntu</title>
        </head>
        <body>
                <h1>Deployed with Ansible by root</h1>
                <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam nisl metus, posuere ac velit eu, rhoncus rutrum magna. Aenean ac dolor quis dolor lacinia rhoncus eget sit amet mi. Nulla metus libero, malesuada vitae tellus ac, porta sollicitudin nisl. In sagittis sagittis diam, at lacinia urna sodales sit amet. Aenean consequat est sit amet interdum rutrum. Nulla vulputate urna ut commodo tempor. Maecenas id pharetra urna, at ultrices dolor. Donec consequat volutpat ipsum. Nam et nulla vitae metus aliquam volutpat vel sit amet turpis. Ut urna mauris, lacinia in mauris ut, sagittis dignissim nunc. Curabitur eu ullamcorper lorem. Suspendisse eu consequat tortor, a ultricies urna. Suspendisse lacinia, libero eget scelerisque sagittis, erat orci sodales dolor, vitae accumsan libero massa ut nunc. Nulla vitae lectus id est faucibus fermentum. Morbi varius leo quis tellus congue luctus. Mauris porttitor nulla nisi, id sagittis quam ornare eu. </p>
        </body>
</html>

vagrant@Control:~/ansible/training$
```

If you want to read more about template, please follow this [link](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_includes.html)