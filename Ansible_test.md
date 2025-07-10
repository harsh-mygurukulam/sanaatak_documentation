# Ansible Unit Test POC

<img width="1148" height="454" alt="Screenshot from 2025-07-11 02-03-43" src="https://github.com/user-attachments/assets/0997b546-31bd-4677-9c14-41d409bf90cf" />

---
| **Author** | **Created On** | **Version** | **Last Edited** | **Review Level** | **Reviewer** |
|------------|----------------|-------------|------------------|------------------|--------------|
| Shivani Narula| *09-07-2025*   | v1.0        | *--*             | L0  | Rajkumar Chauhan |

---

##   Table of Contents

1. [Objective](#objective)
2. [Tools Required](#tools-required)
3. [Directory Structure](#directory-structure)
4. [Playbook](#playbook)
5. [⚙Molecule Configuration](#molecule-configuration)
6. [Converge Playbook](#converge-playbook)
7. [Testinfra Tests](#testinfra-tests)
8. [Run the POC](#run-the-poc)
9. [Conclusion](#conclusion)
10. [Contact](#contact)
11. [References](#references)

---

##   Objective

This POC demonstrates how to unit test an Ansible playbook using:
- **Molecule** (for scenario management),
- **Docker** (to simulate target systems), and
- **Testinfra** (for writing validation tests in Python).

The goal is to ensure:
- The playbook runs without errors
- Server state is validated
- Code follows Ansible best practices

---

## 🛠 Tools Required

| Tool          | Purpose                         | Installation |
|---------------|----------------------------------|--------------|
| Python 3.6+   | For running Molecule & Testinfra | [Python.org](https://www.python.org/downloads/) |
| pip           | Python package manager          | `sudo apt install python3-pip` |
| Ansible       | Configuration management         | `pip install ansible` |
| Docker        | Creates test containers          | [Docker Setup](https://docs.docker.com/get-docker/) |
| Molecule      | Test framework for Ansible       | `pip install molecule[docker]` |
| Testinfra     | Python test library for servers  | `pip install testinfra` |
| Ansible-Lint  | Linter for playbooks             | `pip install ansible-lint` |

---

##   Directory Structure
```bash
ansible-poc-playbook/
├── playbook.yml
├── molecule/
│ └── default/
│ ├── converge.yml
│ ├── molecule.yml
│ └── tests/
│ └── test_playbook.py
```

<img width="1194" height="497" alt="Screenshot from 2025-07-11 02-08-17" src="https://github.com/user-attachments/assets/639ec735-45f0-47fd-94fb-1aeb644afd42" />

---


##   Playbook

**File:** `playbook.yml`

```yaml
- name: Install and configure Apache
  hosts: all
  become: yes
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Apache
      apt:
        name: apache2
        state: present

    - name: Start Apache service
      service:
        name: apache2
        state: started
        enabled: yes
      when: ansible_virtualization_type != "docker"

    - name: Skip service start in Docker
      debug:
        msg: "Skipping start inside Docker"
      when: ansible_virtualization_type == "docker"
```

<img width="1194" height="497" alt="Screenshot from 2025-07-11 02-10-42" src="https://github.com/user-attachments/assets/a7c0fc9c-7018-4fcf-a136-b866dcaadf59" />


---

##  Molecule Configuration
 File: molecule/default/molecule.yml

---
```yaml
dependency:
  name: galaxy

driver:
  name: docker

platforms:
  - name: instance
    image: geerlingguy/docker-ubuntu2004-ansible
    pre_build_image: true

provisioner:
  name: ansible
  playbooks:
    converge: converge.yml

verifier:
  name: testinfra
```

<img width="1204" height="329" alt="Screenshot from 2025-07-11 02-22-25" src="https://github.com/user-attachments/assets/5438e498-0499-4d0c-a127-996b4c0c8dfd" />

---

##  Converge Playbook
File: molecule/default/converge.yml

```yaml
- name: Apply main playbook
  hosts: all
  become: yes
  tasks:
    - import_playbook: ../../playbook.yml
```

<img width="1206" height="94" alt="Screenshot from 2025-07-11 02-24-21" src="https://github.com/user-attachments/assets/d86530f5-163a-4178-aceb-70a495930d40" />

---

## Testinfra Tests
File: molecule/default/tests/test_playbook.py

```yaml
import os
import testinfra.utils.ansible_runner

testinfra_hosts = testinfra.utils.ansible_runner.AnsibleRunner(
    os.environ["MOLECULE_INVENTORY_FILE"]
).get_hosts("all")


def test_apache_package_installed(host):
    pkg = host.package("apache2")
    assert pkg.is_installed


def test_apache_service_skipped_in_docker(host):
    svc = host.service("apache2")
    assert not svc.is_running
    assert not svc.is_enabled
```

<img width="1203" height="295" alt="Screenshot from 2025-07-11 02-26-04" src="https://github.com/user-attachments/assets/a1afe34f-232b-4b99-a6e0-3e88a9661ddc" />

---

## Run The Molecules test
```bash
molecule test
```
This runs:
| **Step**     | **Description**                                          |
|--------------|----------------------------------------------------------|
| `lint`       | Runs Ansible linting checks (syntax + style)             |
| `create`     | Launches Docker container for testing                    |
| `converge`   | Applies the Ansible playbook to the container            |
| `verify`     | Runs Testinfra test cases to validate outcome            |
| `destroy`    | Destroys/removes the test container                      |

<img width="1306" height="725" alt="Screenshot from 2025-07-11 03-25-12" src="https://github.com/user-attachments/assets/e5e04c67-7d40-4488-9504-cbef37724d3e" />

---

# Conclusion
This POC successfully demonstrates automated unit testing for Ansible playbooks using Molecule and Testinfra.

With this approach:

- You ensure idempotence and correct system state.

- Tests can be integrated into CI/CD pipelines.

- Playbook quality and reliability improve.


# Contact Information 
| Name       | Email Address                |
|------------|------------------------------|
|Shivani Narula     |shivani.narula.snaatak@mygurukulam.co|

# References

| **Link** | **Description** |
|------------------------------------------------------|------------------|
| [Molecule Documentation](https://molecule.readthedocs.io/)%7C Molecule Documentation      |
| [Testinfra Documentation](https://testinfra.readthedocs.io/)%7C Testinfra Documentation   |

