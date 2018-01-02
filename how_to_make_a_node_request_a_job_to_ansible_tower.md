# Two way of pull mode in ansible
In most of case, ansible's push mode is enough to automate a job. but sometimes pull mode is needed to deal with huge amount of nodes.
then, what is the pull mode and push mode?
- push mode: ansible's default architecture which push commands to managed nodes and triger it to action.
- pull mode: chef and puppet's default architecture which each agent fetches the command from control node and execute it.

Traditionally Ansible is run in "push" mode where Ansible playbooks are run from a well known machine or from local machine and this, in turn, makes changes on remote machines as per the inventory.

So I'll show you the two way of pull mode feature. 
  1. ansible-pull
  1. ansible tower's callback.
## ansible-pull
### create git repo for ansible playbook repository
my sample repository is here: https://github.com/hatsari/ansible-pull-sample.

### write down sample playbook to execute
  - filename: ansible-pull.yml
```yaml
---
- name: ansible pull sample
  hosts: localhost
  connection: local
  tasks:
    - name: print message
      debug:
        msg: "print ansible pull"
    - name: create text file to verify the action
      copy:
        content: |
          this file is generated by ansible
          copy module is used
        dest: ./file_from_ansible_pull.txt
        mode: 0400
```

### create inventory file
  - filename: hosts
```yaml
localhost connection=local
```

### upload the files to git repository
  - playbook file: ansible-pull.yml
  - inventory file: hosts
  
```shell
vm> ls 
ansible-pull.yml     hosts

vm> git add ./ ; git commit -m "changed"; git push
[master 434f05e] changed
 1 file changed, 1 insertion(+), 1 deletion(-)
 Counting objects: 3, done.
 Delta compression using up to 4 threads.
 Compressing objects: 100% (3/3), done.
 Writing objects: 100% (3/3), 301 bytes | 0 bytes/s, done.
 Total 3 (delta 2), reused 0 (delta 0)
 remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
 To github.com:hatsari/article.git
    4a1751d..434f05e  master -> master
```
### execute on node
basic ansible-pull format is here.
```shell
vm> ansible-pull -d <destination path> -U <git url> [playbook.yml]
```

if playbook file is not specified, it will run 'local.yml' or $hostname.yml

----
*Caution: you must specify full directory path as destination path(-d) . if not, you will encount an error*
```shell
ERROR! the playbook: project/ansible-pull.yml could not be found

```
----

```shell
vm> ansible-pull -d /tmp/project -U https://github.com/hatsari/ansible-pull-sample.git -i /tmp/project/hosts ansible-pull.yml
```

### verify result
```shell
cat /tmp/project/file_from_ansible_pull.txt
----
this file is generated by ansible
copy module is used
```
## ansible tower's callback