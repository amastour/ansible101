# Ansible Template
Managing configuration of multiple machines and environement are one of use cases of Ansible. but these configurations files my vary of each server or each environement, different in some few parameters, but the other settings are same.

Creating static file for each of these  configiration is not good idea, so the best way is to create a template with dynamic value that we can manage it.

A template in Ansible is a file which contains all your configuration parameters, but the dynamic values are given as variables. During the playbook execution, depending on the conditions like which cluster you are using, the variables will be replaced with the relevant values.

Let's take our previous playbook that we have used to install Nginx, this time, we'll create a template named index.html.j2 (j2 is an extention of jinja2 )
```html
<!DOCTYPE html>
<html>
	<body>
		<h1>Deployed with Ansible by {{ ansible_user_id }}</h1>
		<p>{{ data }} </p>
	</body>
</html>
```
After we'll add variables  task to use this template
```yaml
- name: add index.html
  template:
        src: index.html.j2
        dest: /var/www/html/index.html
```
Now run this playbook:
```shell
vagrant@Control:~/ansible/training$ ansible-playbook -i inventory/hosts lesson-06-templates/lesson-06.yml -b -v
Using /etc/ansible/ansible.cfg as config file

PLAY [front] *************************************************************************************************************
TASK [Gathering Facts] *************************************************************************************************
ok: [loadbalancing]

TASK [Install the Nginx HTTP Server] *********************************************************************************** 
[WARNING]: Could not find aptitude. Using apt-get instead

ok: [loadbalancing] => {"cache_update_time": 1566578803, "cache_updated": true, "changed": false}

TASK [create html directory] *******************************************************************************************
ok: [loadbalancing] => {"changed": false, "gid": 0, "group": "root", "mode": "0755", "owner": "root", "path": "/var/www/html", "size": 4096, "state": "directory", "uid": 0}

TASK [add index.html] **************************************************************************************************
changed: [loadbalancing] => {"changed": true, "checksum": "9c2a0971690239556dd87547109485bab296e6fb", "dest": "/var/www/html/index.html", "gid": 0, "group": "root", "md5sum": "b9245c8667c6f58d38a0a684240cf9e6", "mode": "0644", "owner": "root", "size": 1052, "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1566567312.82-206404361563966/source", "state": "file", "uid": 0}

TASK [debug] ***********************************************************************************************************
ok: [loadbalancing] => {
    "msg": "Nginx version nginx installed successfully"
}

PLAY RECAP *************************************************************************************************************
loadbalancing              : ok=5    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

vagrant@Control:~/ansible/training$
```
Let's verify that the index page is added:
```shell
vagrant@Control:~/ansible/training$ curl http://192.168.33.30
<!DOCTYPE html>
<html>
        <body>
                <h1>Deployed with Ansible by root</h1>
                <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam nisl metus, posuere ac velit eu, rhoncus rutrum magna. Aenean ac dolor quis dolor lacinia rhoncus eget sit amet mi. Nulla metus libero, malesuada vitae tellus ac, porta sollicitudin nisl. In sagittis sagittis diam, at lacinia urna sodales sit amet. Aenean consequat est sit amet interdum rutrum. Nulla vulputate urna ut commodo tempor. Maecenas id pharetra urna, at ultrices dolor. Donec consequat volutpat ipsum. Nam et nulla vitae metus aliquam volutpat vel sit amet turpis. Ut urna mauris, lacinia in mauris ut, sagittis dignissim nunc. Curabitur eu ullamcorper lorem. Suspendisse eu consequat tortor, a ultricies urna. Suspendisse lacinia, libero eget scelerisque sagittis, erat orci sodales dolor, vitae accumsan libero massa ut nunc. Nulla vitae lectus id est faucibus fermentum. Morbi varius leo quis tellus congue luctus. Mauris porttitor nulla nisi, id sagittis quam ornare eu. </p>
        </body>
</html>

vagrant@Control:~/ansible/training$
```

If you want to read more about template, please follow this [link](https://docs.ansible.com/ansible/latest/modules/template_module.html)