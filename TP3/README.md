# TP2 Devops Ansible

## Introduction

Command:

```bash
ansible all -i inventories/setup.yml -m ping
```

Response:

```json
leon.dumestre.takima.cloud | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```

---

Command:

```bash
ansible all -i inventories/setup.yml -m setup -a "filter=ansible_distribution*"
```

Response:

```json
leon.dumestre.takima.cloud | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "CentOS",
        "ansible_distribution_file_parsed": true,
        "ansible_distribution_file_path": "/etc/redhat-release",
        "ansible_distribution_file_variety": "RedHat",
        "ansible_distribution_major_version": "7",
        "ansible_distribution_release": "Core",
        "ansible_distribution_version": "7.9",
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false
}
```

---

Command:

```bash
ansible all -i inventories/setup.yml -m yum -a "name=httpd state=absent" --become
```

Response:

```json
leon.dumestre.takima.cloud | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "msg": "",
    "rc": 0,
    "results": [
        "httpd is not installed"
    ]
}
```

## Playbooks

Command:

```bash
ansible-playbook -i inventories/setup.yml playbook-ping.yml
```

Response:

```txt
PLAY [all]

TASK [Test connection] 
ok: [leon.dumestre.takima.cloud]

PLAY RECAP
leon.dumestre.takima.cloud : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

Command:

```bash
ansible-playbook -i inventories/setup.yml playbook-docker-install.yml
```

Response:

```txt
PLAY [all]

TASK [Install device-mapper-persistent-data]
changed: [leon.dumestre.takima.cloud]

TASK [Install lvm2]
changed: [leon.dumestre.takima.cloud]

TASK [add repo docker]
[WARNING]: Consider using 'become', 'become_method', and 'become_user' rather than running sudo
changed: [leon.dumestre.takima.cloud]

TASK [Install Docker]
changed: [leon.dumestre.takima.cloud]

TASK [Install python3]
changed: [leon.dumestre.takima.cloud]

TASK [Install docker with Python 3]
changed: [leon.dumestre.takima.cloud]

TASK [Make sure Docker is running]
changed: [leon.dumestre.takima.cloud]

PLAY RECAP
leon.dumestre.takima.cloud : ok=7    changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

---

Command:

```bash
ansible-galaxy init roles/docker
```

Response:

```txt
- Role roles/docker was created successfully
```

## Deploy app

```bash
ansible-playbook -i inventories/setup.yml playbook-deploy.yml
```
