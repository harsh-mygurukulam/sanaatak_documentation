# Ansible Role: MyRole (POC with Molecule & Docker)

This POC shows how to write an Ansible role and test it using Molecule with the Docker driver.

---

## 🛠️ What This Role Does

- Installs Apache2
- Starts and enables the Apache service (skips in Docker containers)

---

## 📁 Folder Structure

```bash
ansible-poc-playbook/
├── playbook.yml
└── roles/
    └── myrole/
        ├── tasks/
        │   └── main.yml
        ├── molecule/
        │   └── default/
        │       ├── converge.yml
        │       └── molecule.yml
        ├── meta/
        │   └── main.yml
        └── README.md

```
![Screenshot from 2025-07-09 02-51-24](https://github.com/user-attachments/assets/05c8bac7-e48c-4af3-a048-5a7829f7d090)

---


## Install Required Tools

```bash
sudo apt update
sudo apt install docker.io python3-pip python3-venv -y

# Create and activate a virtual environment
python3 -m venv myenv
source myenv/bin/activate

# Install Ansible and Molecule
pip install ansible molecule[docker]
```

## Molecule Configuration
## molecule/default/molecule.yml
```bash
---
dependency:
  name: galaxy

driver:
  name: docker

platforms:
  - name: instance
    image: "geerlingguy/docker-ubuntu2204-ansible"
    command: /lib/systemd/systemd
    privileged: true
    pre_build_image: true

provisioner:
  name: ansible

verifier:
  name: ansible

```


![Screenshot from 2025-07-09 18-04-30](https://github.com/user-attachments/assets/69f2f4ac-84c6-403c-9523-d92fd71d7da9)



## Role Code
## tasks/main.yml
```bash
---
- name: Update apt cache
  apt:
    update_cache: yes

- name: Install Apache
  apt:
    name: apache2
    state: present

- name: Start Apache (unless inside Docker)
  service:
    name: apache2
    state: started
    enabled: yes
  when: ansible_virtualization_type != "docker"

- name: Message when running in Docker
  debug:
    msg: "Skipping start/enable because running inside Docker"
  when: ansible_virtualization_type == "docker"


```

## meta/main.yml
```bash
#SPDX-License-Identifier: MIT-0
```
---


![Screenshot from 2025-07-09 18-11-46](https://github.com/user-attachments/assets/77cf7bef-5d03-4aa1-81d3-0289e132ff34)

## Running the Molecule Test
## Run the following command from inside your role folder:
```bash
cd ansible-poc-playbook/roles/myrole
molecule test

```
![Screenshot from 2025-07-09 19-04-16](https://github.com/user-attachments/assets/90a644e3-9856-4cba-bbc8-d6dc5714c7de)




![Screenshot from 2025-07-09 19-08-30](https://github.com/user-attachments/assets/15a41343-9568-4d68-8aa4-fda1501619f5)


