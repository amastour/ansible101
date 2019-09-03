# Ansible Playbook
Playbook in Ansible is a simple text file that describes configuration which can be used to configure a remote server. Playbooks are really simple but very powerful; they are written in YAML format and the tasks mentioned inside the playbook are execute sequentially. YAML is human-readable data serialization format, which cares about whitespace like Python, it represents tree-like structure. 

* If you want to read more about YAML, please refer to this [link](http://www.yaml.org/spec/1.2/spec.html).

### Nginx Package Installation Example:
Lets start to build the playbook which will install the **nginx** HTTP server on the machines inside the `front` group.
```yaml
---
- hosts: front
  become: yes
  gather_facts: yes
  tasks:
    - name: Install the Nginx HTTP Server
      apt: 
        name: nginx
        update_cache: yes 
        state: present
```
 Here is the explainaiton of this playbook:

`hosts: front` - Name of the host/group on which this playbook should execute.
`become: yes` - If it's `yes` then always use `sudo` to execute each task of this playbook.
`gather_facts: yes` - Connects to the remote host for getting the info like CPU arch, OS, IP addresses and more.
`tasks` - List of tasks to execute in this playbook.
We just have one task that use the [apt](http://docs.ansible.com/ansible/apt_module.html) module to install the `nginx` package and also mentioned to update the package cache. We also added the `name` to this task, which is not mandatory but highly recommended. 

If we want to see the list of the hosts on which this playbook will make change, then we can use the `--list-hosts` command:
```shell
vagrant@Control:~/ansible$ ansible-playbook  -i training/inventory/hosts training/lesson-03/lesson-03.yml --list-hosts
 [WARNING] Ansible is being run in a world writable directory (/home/vagrant/ansible), ignoring it as an ansible.cfg source. For more information see https://docs.ansible.com/ansible/devel/reference_appendices/config.html#cfg-in-world-writable-dir

playbook: training/lesson-03/lesson-03.yml

  play #1 (front): front    TAGS: []
    pattern: [u'front']
    hosts (1):
      loadbalancing

vagrant@Control:~/ansible$
```
Now it's time to execute this playbooks and for this we will be using `ansible-playbook`:
```shell
ansible-playbook -i training/inventory/hosts lesson-03.yml
```
Output will give you the details of each task whether succeeded, failed or has been made any-change. Here is how the console output should look like:
```shell
vagrant@Control:~/ansible$ ansible-playbook  -i training/inventory/hosts training/lesson-03/lesson-03.yml

PLAY [front] *************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [loadbalancing]

TASK [Install the Nginx HTTP Server] ***********************************************************************************
changed: [loadbalancing]

PLAY RECAP *************************************************************************************************************
loadbalancing              : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

vagrant@Control:~/ansible$
```
Let's talk about the each line of the output.
```shell
PLAY [front] ********************************************************************
```
Ansible let us know that it is running the play on hosts `front`.
```shell
GATHERING FACTS ***************************************************************
ok: [loadbalancing]
```
Beginning of each play, Ansible run `setup` module on remote hosts to gather facts, if we don't want to gather the fact then we can just change `gather_facts: no`.
```shell
TASK: [Install the Nginx HTTP Server] *****************************************
changed: [loadbalancing]
```
Next, our only task ran on the host `loadbalancing` and report that it has changed something on the host (it this case, it has installed Nginx).

At the end, Ansible gives the summary of what happened: two tasks have been run, one of them has made change on the target host (Install Nginx) and setup module doesn't make any change.

Let's try to run this playbook once again:
```shell
vagrant@Control:~/ansible$ ansible-playbook  -i training/inventory/hosts training/lesson-03/lesson-03.yml

PLAY [front] *************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [loadbalancing]

TASK [Install the Nginx HTTP Server] ***********************************************************************************
ok: [loadbalancing]

PLAY RECAP *************************************************************************************************************
loadbalancing              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

vagrant@Control:~/ansible$
```
Changed is `0` now because tasks that were successful run before **will-not** make any change on the host again, meaning they will make only change when that are required to bring the target host into the desired state. This concept is called idempotent, meaning change commands are not run unless needed. In other words, on each execution of this playbook, system will not install the Nginx.

This playbook is not completed, we'll work on it during the next lessons. If you want to read more about playbook, please refer this [link](http://docs.ansible.com/ansible/playbooks.html).
