# TP3 Devops Ansible

## Introduction

This command ping the server thanks to [setup.yml](ansible/inventories/setup.yml) file.
If it's a success, the server answer `pong`.

```bash
ansible all -i inventories/setup.yml -m ping
```

- `all`: all the machines in the inventory
- `-i inventories/setup.yml` : inventory file
- `-m ping`: module to use

---

Command:

```bash
ansible all -i inventories/setup.yml -m setup -a "filter=ansible_distribution*"
```

- `all`: all the machines in the inventory
- `-i inventories/setup.yml` : inventory file
- `-m setup`: module to use
- `-a "filter=ansible_distribution*"` : argument to send to the module. Here, it's a filter used to display only the distribution of the machines

---

```bash
ansible all -i inventories/setup.yml -m yum -a "name=httpd state=absent" --become
```

- `all`: all the machines in the inventory
- `-i inventories/setup.yml` : inventory file
- `-m yum`: module to use
- `-a "name=httpd state=absent"` : argument to send to the module. The module https will be removed if it exists.

## Playbooks

This command execute playbook-ping.yml to test connection.

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

---

A role can be used to divide playbook execution. This command lets you add role folders locally.

```bash
ansible-galaxy init roles/docker
```

## Deploy app

```bash
ansible-playbook -i inventories/setup.yml playbook-deploy.yml
```

All tasks are splited between multiple roles for efficient deployment:

- install docker
- create network
- launch database
- launch app
- launch front
- launch proxy

`Launch` playbooks use images published on Docker Hub.

Global variables are stored in the [all.yml](ansible/group_vars/all.yml) file and used in the various roles.

## Containers

### Backend API & Database

Backend API and database containers are the same than [TP1](/TP1/README.md).

### Front

The Front displays an interface and calls the backend API endpoints.
The code comes from this [repository](https://github.com/takima-training/devops-front).

### Proxy

The purpose of the proxy is to redirect to the right containers, i.e. the backend and frontend. It enables links to be customized and load balancing to be set up.

When you go to the default server link, the proxy redirects to the frontend. If you add `/api` after the link, the proxy redirects to the backend.

## Github Actions

To deploy images efficiently on the server, we'll use Github Actions. When we push on the `main` branch, Github will [launch the tests](/.github/workflows/test-backend.yml), then [build and publish the images](/.github/workflows/image.yml), and finally [publish on the server](/.github/workflows/deploy.yml). If any of the pipelines fail, the actions are stopped.

To deploy on the server, I use [dawidd6/action-ansible-playbook](https://github.com/marketplace/actions/run-ansible-playbook). SSH key and server url are stored as secret on Github.
