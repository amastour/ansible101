# Ansible Ad-hoc Commands
Login to the **control** (machine on which the ansible is installed), by using the following command:
```
vagrant ssh
```
Check that the ansible is properly installed on it:
```
ansible --version
```
```shell
vagrant@Control:~/ansible$ ansible --version
 [WARNING] Ansible is being run in a world writable directory (/home/vagrant/ansible), ignoring it as an ansible.cfg source. For more information see https://docs.ansible.com/ansible/devel/reference_appendices/config.html#cfg-in-world-writable-dir
ansible 2.8.4
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/vagrant/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.15+ (default, Nov 27 2018, 23:36:35) [GCC 7.3.0]
vagrant@Control:~/ansible$
```
Now come to the Ansible Ad-hoc commands, In simple words, ad-hoc commands are those commands which you want to execute directly without writing a playbook.

**Example-1#** Now that we have verified that the ansible is installed, let's also verify that everything is working properly and for this we'll use `ping` module, it will run against all the hosts that are in our hosts file.
```
ansible -m ping all
```
`ping` module always returns `pong` on successful contact.
```json
vagrant@Control:~/ansible$ ansible -i training/inventory/hosts -m ping all
database | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
web1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
web2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
**Example-2#** Change the remote **hostname** using the `command` module:
```
ansible -m command -a "hostname" all
```
And response we get from server will be:
```json
vagrant@Control:~/ansible$ ansible -i training/inventory/hosts -m command -a "hostname" all

database | CHANGED | rc=0 >>
database

web2 | CHANGED | rc=0 >>
web2

web1 | CHANGED | rc=0 >>
web1

vagrant@Control:~/ansible$
```
**Example-3#** Make a directory on remote servers:
```
ansible -i training/inventory/hosts apache -m file -a "dest=/tmp/test state=directory"
```
The output of the above command will be like this:
```json
vagrant@Control:~/ansible$ ansible -i training/inventory/hosts apache -m file -a "dest=/tmp/test state=directory"
web.example.com | success >> {
    "changed": true,
    "gid": 1000,
    "group": "vagrant",
    "mode": "0775",
    "owner": "vagrant",
    "path": "/tmp/test",
    "size": 4096,
    "state": "directory",
    "uid": 1000
}

vagrant@Control:~/ansible$
```
If you need more explanation about the ad-hoc command, please refer this [link](http://docs.ansible.com/ansible/intro_adhoc.html).
