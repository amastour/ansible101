Ansible Inventory File:
--------------------------
Ansible can work with multiple systems that you have in your infrastructure by using an INI format file, which is called Inventory file in ansible. During installation, it creates an example that you can use for your reference which is located at `/etc/ansible/hosts`.

We can create our own inventory file from scratch like we did [this](https://gitlab.com/mastour-anas/ansible101/blob/master/training/inventory/hosts). The format of the file is really simple; things in brackets are called group names.
```ini
[apache]
web1
web2

[mysql]
database
```
If `web1` `web2` and `database` are resolvable through DNS, then we are good otherwise we need to add the ip address explicitly for each host.
```ini
[apache]
web1 ansible_host=192.168.33.100
web2 ansible_host=192.168.33.101

[mysql]
db ansible_host=192.168.33.200
```
For detail explainiation please check the official [link](http://docs.ansible.com/ansible/intro_inventory.html).

Thanks

