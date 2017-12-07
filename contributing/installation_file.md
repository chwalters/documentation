[install.yml file]

**-------**


The installation file, called install.yml, must be placed at the root of a repository. This one will be read by Intelora in order to install your module from the [command line](../intelora_cli.md) by any Intelora user.

[How to create an install.yml file]

The module installation process is based on the Ansible program. Ansible is an IT orchestrator. It means it can help you to perform configuration management, application deployment or task automation.

The `install.yml` file must contains what we called a Playbook in the Ansible world.
A playbook is like a recipe or an instructions manual which tells Ansible what to do against an host. In our case, the host will be the local machine of the current user who asked Intelora to install the module.

See a basic playbook, the one used by the neuron [wikipedia_searcher](https://github.com/intelora-project/intelora_neuron_wikipedia)

```yml
- name: Intelora wikipedia_searcher neuron install
  hosts: localhost
  gather_facts: no
  connection: local
  become: true

  tasks:
    - name: "Install pip dependencies"
      pip:
        name: wikipedia
        version: 1.4.0
```

As the file is a **playbook**, it can contains multiple **play**. That's why the file start with a "-", the yaml syntax to define a list of element. In the example, our playbook contains only one play.

The first element is the `name`. It can be anything the user wants. 
Here, we've set what the play do.

The `hosts` parameter is, like the name suggest us, to design on which host we want to apply our configuration. In the context of an Intelora module installation, it will always be **localhost**.

By default, ansible call a module to [gather useful variables] (http://docs.ansible.com/ansible/setup_module.html) about remote hosts that can be used in playbooks.
In this example, we don't need it and so we disable the `gather_facts` feature in order to win a couple seconds during the installation process.

In most of case, our play will need to apply admin operations. In this case, installing a python lib. So we set `become` to true to be allowed to install our lib as root user.

The next part is `tasks`. This key must contains a list of task to apply on the target system.

The only task we've added here is based on the [pip Ansible module] (http://docs.ansible.com/ansible/pip_module.html).

Ansible comes with a lot of modules, see the [complete list] (http://docs.ansible.com/ansible/modules_by_category.html).

Here is an example which use the [apt module] (http://docs.ansible.com/ansible/apt_module.html) to install Debian packages

```yml
tasks:
  - name: Install packages
    apt: name={{ item }} update_cache=yes
    with_items:
      - flac
      - mplayer
```
