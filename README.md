# Ansible role: tool.bootstrap_playbook

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Cbz-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: Boilerplate](https://img.shields.io/badge/Tech-Boilerplate-orange)
![Tag: Bootstrap](https://img.shields.io/badge/Tech-Bootstrap-orange)
![Tag: Molecule](https://img.shields.io/badge/Tech-Molecule-orange)

An Ansible role to bootstrap and create other playbooks.

The Playbook Bootstrap role automates the creation of a basic structure for an Ansible playbook. It sets up the necessary folders, files, and configurations to kickstart your playbook development process. Key features of this role include:

Predefined Folder Structure: The role creates a well-organized folder structure, including molecule, tests, and other necessary directories. This ensures consistency and facilitates modular organization of your playbook components.

Code Quality Tools: Configuration files such as .ansible-lint, .ansible.cfg, and .yamllint are included to enforce code quality standards and promote best practices in your playbook development.

Collaboration Support: The inclusion of a CODEOWNERS file enables seamless collaboration and ownership assignment within your Git repository.

Molecule Integration: The role comes preconfigured with Molecule, a testing framework for Ansible. It includes default Molecule scenarios and GitLab CI configuration for streamlined testing and continuous integration.

By utilizing the Playbook Bootstrap role, you can expedite the creation of new Ansible playbooks, maintain code quality, and foster efficient collaboration within your development team.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
bootstrap_playbook__user: "root"
bootstrap_playbook__base_path: "/root"

bootstrap_playbook__meta_author: "Author"
bootstrap_playbook__meta_namespace: "root"
bootstrap_playbook__meta_playbook_name: "my_new_role"
bootstrap_playbook__meta_description: "This is a limited description for the meta."
bootstrap_playbook__meta_company: "Corp"
bootstrap_playbook__meta_license: "MIT"

bootstrap_playbook__tags:
  - "UNIX"

# The driver you use for molecule
bootstrap_playbook__molecule_driver: "docker"

# The path of your future role
bootstrap_playbook__path: "{{ bootstrap_playbook__base_path }}/{{ bootstrap_playbook__meta_namespace }}.{{ bootstrap_playbook__meta_playbook_name }}"

# A list of folder to create
bootstrap_playbook__folders:
  - "assets"
  - "meta"
  - "molecule"
  - "tests"
  - "tests/tower"
  - "tests/inventory"
  - "tests/inventory/group_vars"
  - "tests/inventory/host_vars"
  - "tests/certs"

bootstrap_playbook__molecule_scenarios:
  - { ssl: true, hosts: 3, name: "default", target_group: "local", docker_image: "labocbz/ansibletest-debian-12:latest" }
  - { ssl: true, hosts: 3, name: "cicd-debian-11", target_group: "cicd-debian-11", docker_image: "${NEXUS_DOCKER_GROUP_REGISTRY}/${DOCKER_IMAGE__ANSIBLETEST_DEBIAN_11}" }
  - { ssl: true, hosts: 3, name: "cicd-debian-12", target_group: "cicd-debian-12", docker_image: "${NEXUS_DOCKER_GROUP_REGISTRY}/${DOCKER_IMAGE__ANSIBLETEST_DEBIAN_12}" }
  - { ssl: true, hosts: 3, name: "cicd-ubuntu-22", target_group: "cicd-ubuntu-22", docker_image: "${NEXUS_DOCKER_GROUP_REGISTRY}/${DOCKER_IMAGE__ANSIBLETEST_UBUNTU_22}" }

bootstrap_playbook__inventory_groups:
  - "SQL"
  - "APACHE2"
  - "PHP"

bootstrap_playbook__requirements:
  - { name: "labocbz.prepare_host", src: "https://github.com/CBZ-D-velop/Ansible-Role-Labocbz-Prepare-Host.git" }

bootstrap_playbook__platforms:
    - "Debian"
    - "Ubuntu"

bootstrap_playbook__molecule_test_sequence:
  - "destroy"
  - "syntax"
  - "dependency"
  - "create"
  - "prepare"
  - "converge"
  - "idempotence"
  - "verify"
  - "destroy"

# Some file to import in the root folder of the future role
bootstrap_playbook__configuration_files:
  - ".ansible-lint"
  - ".ansible.cfg"
  - ".yamllint"
  - "CODEOWNERS"
  - ".gitlab-ci.yml"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_bootstrap_playbook__user: "root"
inv_bootstrap_playbook__base_path: "/root"

inv_bootstrap_playbook__meta_author: "Author"
inv_bootstrap_playbook__meta_namespace: "root"
inv_bootstrap_playbook__meta_playbook_name: "my_new_role"
inv_bootstrap_playbook__meta_description: "This is a limited description for the meta."
inv_bootstrap_playbook__meta_company: "Corp"
inv_bootstrap_playbook__meta_license: "MIT"

inv_bootstrap_playbook__tags:
  - "UNIX"

# The driver you use for molecule
inv_bootstrap_playbook__molecule_driver: "docker"

# The path of your future role
inv_bootstrap_playbook__path: "{{ inv_bootstrap_playbook__base_path }}/{{ inv_bootstrap_playbook__meta_namespace }}.{{ inv_bootstrap_playbook__meta_playbook_name }}"

# A list of folder to create
inv_bootstrap_playbook__folders:
  - "assets"
  - "meta"
  - "molecule"
  - "tests"
  - "tests/tower"
  - "tests/inventory"
  - "tests/inventory/group_vars"
  - "tests/inventory/host_vars"
  - "tests/certs"

inv_bootstrap_playbook__molecule_scenarios:
  - { ssl: true, hosts: 3, name: "default", target_group: "local", docker_image: "labocbz/ansibletest-debian-12:latest" }
  - { ssl: true, hosts: 3, name: "cicd-debian-11", target_group: "cicd-debian-11", docker_image: "${NEXUS_DOCKER_GROUP_REGISTRY}/${DOCKER_IMAGE__ANSIBLETEST_DEBIAN_11}" }
  - { ssl: true, hosts: 3, name: "cicd-debian-12", target_group: "cicd-debian-12", docker_image: "${NEXUS_DOCKER_GROUP_REGISTRY}/${DOCKER_IMAGE__ANSIBLETEST_DEBIAN_12}" }
  - { ssl: true, hosts: 3, name: "cicd-ubuntu-22", target_group: "cicd-ubuntu-22", docker_image: "${NEXUS_DOCKER_GROUP_REGISTRY}/${DOCKER_IMAGE__ANSIBLETEST_UBUNTU_22}" }

inv_bootstrap_playbook__inventory_groups:
  - "SQL"
  - "APACHE2"
  - "PHP"

inv_bootstrap_playbook__requirements:
  - { name: "labocbz.prepare_host", src: "https://github.com/CBZ-D-velop/Ansible-Role-Labocbz-Prepare-Host.git" }

inv_bootstrap_playbook__platforms:
    - "Debian"
    - "Ubuntu"

inv_bootstrap_playbook__molecule_test_sequence:
  - "destroy"
  - "syntax"
  - "dependency"
  - "create"
  - "prepare"
  - "converge"
  - "idempotence"
  - "verify"
  - "destroy"

# Some file to import in the root folder of the future role
inv_bootstrap_playbook__configuration_files:
  - ".ansible-lint"
  - ".ansible.cfg"
  - ".yamllint"
  - "CODEOWNERS"
  - ".gitlab-ci.yml"

```

```YAML
# From AWX / Tower
---
all vars from to put/from AWX / Tower
```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include tool.bootstrap_playbook"
  tags:
    - "tool.bootstrap_playbook"
  vars:
    bootstrap_playbook__user: "{{ inv_bootstrap_playbook__user }}"
    bootstrap_playbook__base_path: "{{ inv_bootstrap_playbook__base_path }}"
    bootstrap_playbook__meta_author: "{{ inv_bootstrap_playbook__meta_author }}"
    bootstrap_playbook__meta_namespace: "{{ inv_bootstrap_playbook__meta_namespace }}"
    bootstrap_playbook__meta_role_name: "{{ inv_bootstrap_playbook__meta_role_name }}"
    bootstrap_playbook__meta_description: "{{ inv_bootstrap_playbook__meta_description }}"
    bootstrap_playbook__meta_company: "{{ inv_bootstrap_playbook__meta_company }}"
    bootstrap_playbook__meta_license: "{{ inv_bootstrap_playbook__meta_license }}"
    bootstrap_playbook__tags: "{{ inv_bootstrap_playbook__tags }}"
    bootstrap_playbook__molecule_driver: "{{ inv_bootstrap_playbook__molecule_driver }}"
    bootstrap_playbook__path: "{{ inv_bootstrap_playbook__path }}"
    bootstrap_playbook__folders: "{{ inv_bootstrap_playbook__folders }}"
    bootstrap_playbook__molecule_scenarios: "{{ inv_bootstrap_playbook__molecule_scenarios }}"
    bootstrap_playbook__inventory_groups: "{{ inv_bootstrap_playbook__inventory_groups }}"
    bootstrap_playbook__requirements: "{{ inv_bootstrap_playbook__requirements }}"
    bootstrap_playbook__platforms: "{{ inv_bootstrap_playbook__platforms }}"
    bootstrap_playbook__molecule_test_sequence: "{{ inv_bootstrap_playbook__molecule_test_sequence }}"
    bootstrap_playbook__configuration_files: "{{ inv_bootstrap_playbook__configuration_files }}"
  ansible.builtin.include_role:
    name: "tool.bootstrap_playbook"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-05-08: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-05-30: Cryptographic update

* SSL/TLS Materials are not handled by the role
* Certs/CA have to be installed previously/after this role use

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2024-02-20: Remastered

* Imported new CICD
* Rework global on readme
* Rename of vars __

### 2024-03-01: Rework

* You can now define your scenario
* Groups vars handled
* You can now define your requirements
* You can now define your test sequence
* You can now define your configuration list file
* You can now define docker images for your scenario
* You can now define how much hosts to create for each scenarios

### 2024-05-19: New CI

* Added Markdown lint to the CICD
* Rework all Docker images
* Change CICD vars convention
* New workers
* Removed all automation based on branch

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
