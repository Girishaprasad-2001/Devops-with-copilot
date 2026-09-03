# Ansible A to Z Commands & Playbooks Cheat Sheet

A complete Ansible reference covering beginner, intermediate, and advanced commands used by DevOps Engineers, System Administrators, SREs, Cloud Engineers, and CI/CD professionals.

---

# A. Ansible Installation

## Check Ansible Version

```bash
ansible --version
```

## Check Configuration

```bash
ansible-config view
```

## List All Configurations

```bash
ansible-config list
```

---

# B. Basic Commands

## Ping All Hosts

```bash
ansible all -m ping
```

## Ping Specific Host Group

```bash
ansible webservers -m ping
```

## Gather Facts

```bash
ansible all -m setup
```

---

# C. Configuration Commands

## View Current Config

```bash
ansible-config dump
```

## Initialize Config

```bash
ansible-config init --disabled > ansible.cfg
```

---

# D. Debug Module

## Print Variable

```bash
ansible localhost -m debug -a "msg=Hello World"
```

### Playbook Example

```yaml
---
- hosts: all
  tasks:
    - debug:
        msg: "Deployment Started"
```

---

# E. Execute Commands

## Run Shell Command

```bash
ansible all -m shell -a "uptime"
```

## Run Command Module

```bash
ansible all -m command -a "df -h"
```

---

# F. Facts Gathering

## Collect System Facts

```bash
ansible all -m setup
```

## Display OS Information

```bash
ansible all -m setup -a "filter=ansible_distribution*"
```

*--

# G. Groups & Inventory

## In*entory File Example

```ini*[*ebservers]
host1
host*

[dbservers]
host3*host4
```

*#*List Inventory Hosts

```bash**nsible-inventory --list
```

*# Inventory*Graph

```bash*ansible-inventory --graph
```

---*
# H. Host Management

## Check Co*nectivity

```bash*ansible all -m ping
```

*# Limit Command to Host*
```bash*ansible all -m ping*--limit host1
```

---

# I. Inven*ory Commands

*# List Inventory

```bash*ansible-inventory -i inventory*ini --list
```

*# Show Graph

```bash*ansible-inventory --graph*```

---

# J. JSON*Output

## JSON*Inventory

*``bash
ansible-inventory --list
``*

## JSON Facts

*``bash*ansible all -m setup
```

*--

# K. Key Management

## Copy*SSH Key

```bash*ssh-copy-id user@server
```

*# Test*SSH Connectivity

```bash*ssh user@server
```

---

# L. Lin*x Package Management

## Install P*ckage Using Yum

```yaml*---
- hosts: webservers*  tasks:
    -*yum:
        name: httpd
        s*ate: present
```

*# Install Package Using Apt

*``yaml*---
- hosts: ubuntu
  tasks:
    -*apt:
        name: nginx
        s*ate: present
```

---

# M. Module*Commands

## List*Documentation

```bash*ansible-doc -l
```

*# Specific Module Documentation

`*`bash
ansible-doc copy
*``

*# Show Module Details

```bash*ansible-doc yum
```

---

# N. Net*ork*Automation*
## Run Command on Cisco Device

`*`yaml
---
- hosts: routers
  gathe*_facts: false

  tasks:
    - ios_*ommand:
        commands:
        * - show version
```

---

# O. Out*ut Control

## Verbose Output

```*ash
ansible-playbook playbook.yml *v
```

## Very Verbose

```bash
an*ible-playbook playbook.yml -vvv
``*

---

# P. Playbook Commands

## *xecute Playbook

```bash
ansible-p*aybook playbook.yml
```

## Syntax*Check

```bash
ansible-playbook pl*ybook.yml --syntax-check
```

## D*y Run

```bash
ansible-playbook*playbook.yml --check
```

---

# Q* Quick Ad-Hoc Commands

##*Check*Disk Space*
```bash*ansible all -m shell -a "df -h"
``*

## Check Memory

```bash*ansible all -m shell -a "free*-m"
```

*--

# R. Roles

## Create*Role Structure

```bash*ans*ble-galaxy init apache-role
```

#*# Role Structure

```text
*pache-role/
├── defaults
├── files*├── handlers
├── meta
├── tasks
├─* templates
├── tests
└──*vars
```

---

# S. Service*Management

## Start Service

```y*ml
---
- hosts: all
  tasks:
    -*service:
        name* nginx
        state: started
```
*## Restart Service

```yaml*---
* hosts: all
  tasks:
    - service*
        name: nginx
        state* restarted
```

---

# T. Template*Management

## Jinja2 Template

``*j2
Server*ame {{ inventory_hostname*}}
```

### Template Task

```yaml*- template:
    src: apache.conf.j*
    dest: /etc/httpd/conf/httpd.c*nf
```

---

# U. User Management
*## Create User

```yaml
---
- host*: all
  tasks:
    - user:
       *name: devops
        state: presen*
```

## Remove User

```yaml
---
* hosts: all
  tasks:
    - user:
 *      name: devops
        state: *bsent
```

---

# V. Variables

##*Variable Example

```yaml
---
- ho*ts: all

  vars:
    package_name:*nginx

  tasks:
    - yum:
       *name: "{{ package_name }}"
       *state: present
```

---

# W. Wait*Commands

## Wait for Port

```yam*
- wait_for:
    port: 80
    time*ut: 60
```

## Wait for File

```y*ml
- wait_for:
    path: /tmp/app.*og
```

---

# X. Execute Shell Sc*ipts

## Run Script

```yaml
---
-*hosts: all
  tasks:
    - script: *eploy.sh
```

---

# Y. YAML Valid*tion

## Validate Playbook

```bas*
ansible-playbook playbook.yml --s*ntax-check
```

## Lint Playbook

*``bash
ansible-lint playbook.yml
`*`

---

# Z. Advanced Administrati*n

## Encrypt Variable

```bash
an*ible-vault encrypt secrets.yml
```*
## Edit Vault

```bash
ansible-va*lt edit secrets.yml
```

## View V*ult

```bash
ansible-vault view se*rets.yml
```

## Decrypt File

```*ash
ansible-vault decrypt secrets.*ml
```

---

# Ansible Playbook St*ucture

```yaml
---
- name: Sample*Playbook
  hosts: webservers
  bec*me: yes

  tasks:

   *- name: Install Nginx
      packag**
        name: nginx
        state* present*
    - name: Start Nginx
      ser*ice:
        name: nginx
        s*ate: started
```

---

# Handlers *xample

```yaml
---
- hosts: webse*vers

  tasks:
    - name: Update *onfig
      copy:
        src: app*conf
        dest: /etc/nginx/ngin*.conf
      notify:
        - Rest*rt Nginx

  handlers:
    - name: *estart Nginx
      service:
      * name: nginx
        state: restar*ed
```

---

# Loops Example

```y*ml
---
- hosts: all

  tasks:
    * name: Install Packages
      pack*ge:
        name: "{{ item }}"
   *    state: present
      loop:
   *    - git
        - nginx
        * docker
```

---

# Conditions Exa*ple

```yaml
---
- hosts: all

  t*sks:
    - name: Install Apache on*RedHat
      yum:
        name: ht*pd
        state: present
      wh*n: ansible_os_family == "RedHat"
`*`

---

# Ansible Vault Example

#* Encrypt File

```bash
ansible-vau*t encrypt passwords.yml
```

## Ru* Playbook with Vault

```bash
ansi*le-playbook site.yml --ask-vault-p*ss
```

---

# Dynamic Inventory E*ample

```bash
ansible-inventory -* aws_ec2.yml --graph
```

---

# C*mmon DevOps Playbook

```yaml
---
* hosts: webservers
  become: yes

* tasks:

    - name: Install Nginx*      package:
        name: nginx*        state: present

    - name* Start Nginx
      service:
      * name: nginx
        state: starte*

    - name: Enable Nginx
      s*rvice:
        name: nginx
       *enabled: yes
```

---

# Top 25 An*ible Commands Used Daily

```bash
*nsible --version
ansible all -m pi*g
ansible all -m setup
ansible-pla*book site.yml
ansible-playbook sit*.yml --check
ansible-playbook site*yml --syntax-check
ansible-invento*y --list
ansible-inventory --graph*ansible-doc -l
ansible-doc yum
ans*ble-galaxy init role-name
ansible *ll -m shell -a "uptime"
ansible al* -m shell -a "df -h"
ansible all -* command -a "hostname"
ansible-pla*book deploy.yml -vvv
ansible-vault*encrypt secrets.yml
ansible-vault *ecrypt secrets.yml
ansible-vault e*it secrets.yml
ansible-vault view *ecrets.yml
ansible-lint playbook.y*l
ssh-copy-id user@host
ansible lo*alhost -m debug -a "msg=test"
ansi*le-playbook site.yml --limit webse*vers
ansible-playbook site.yml --t*gs install
ansible-playbook site.y*l --skip-tags test
```

---

# Ans*ble Troubleshooting Commands

```b*sh
ansible --version
ansible all -* ping
ansible-inventory --list
ans*ble-inventory --graph
ansible-play*ook site.yml --syntax-check
ansibl*-playbook site.yml --check
ansible*playbook site.yml -vvv
ansible all*-m setup
ssh -vvv user@host
ansibl*-lint playbook.yml
```

---

# Com*on Ansible Workflow

```text
Inven*ory
    │
    ▼
Playbook
    │
   *▼
Tasks
    │
    ▼
Modules
    │
*   ▼
Handlers
    │
    ▼
Target S*rvers
```

---

# Summary

This ch*at sheet covers:

- Ansible Instal*ation
- Inventory Management
- Ad-Hoc Commands
- Playbooks
- Roles
- Variables
- Templates
- Handlers
- Loops & Conditions
- Vault Security
- Dynamic Inventory
- Network Automation
- CI/CD Integration
- Linux Administration
- Production Automation

⭐ Keep this README as a complete Ansible quick reference for DevOps, SRE, Platform Engineering, Cloud Engineering, and Automation activities.
