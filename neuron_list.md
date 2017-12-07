[Neuron installation]

**-------**

A Neuron is a module performs some actions attached to an order. This can be used on the synpases.
 
See the complete [neuron documentation] -> (neurons.md) for more information.

[Neuron list]

The user can get the full neuron list available on [the project web site] -> (https://intelora-project.github.io/).

[Installation]

The core neurons are already packaged with the installation of intelora and it can be used out of the box. 

Community neuron need to be installed manually.

[NOTE] 
To install a neuron, `resource_directory` must be declared in the user's [settings] -> (settings.md).

**Via the intelora's CLI**

CLI syntax
```bash
intelora install --git-url <git_url>
```

Example:
```bash
intelora install --git-url https://github.com/intelora-project/intelora_neuron_wikipedia.git
```
You may be asked to type your `sudo` password during the process.

**Manually**

You can also install a neuron manually.

First, clone the repo in the right resource folder. 
```bash
cd /path/to/resource_folder
git clone <plugin_url>
```

Then, install it manually via Ansible (Ansible has been installed with intelora)
```bash
cd <cloned_repo>
ansible-playbook install.yml -K
```

Full example

```bash
cd /home/me/my_intelora_config/resources/neurons
git clone https://github.com/intelora-project/intelora_neuron_hue.git
cd hue
ansible-playbook install.yml -K
```

[Uninstall a resource]

**Via the intelora's CLI**

[NOTE]
To uninstall a resource, `resource_directory` must be decalred in the [settings] -> (settings.md).

CLI syntax

```bash
intelora uninstall --neuron-name <neuron_name>
intelora uninstall --stt-name <stt_name>
intelora uninstall --tts-name <tts_name>
intelora uninstall --trigger-name <trigger_name>
```

Example:

```bash
intelora uninstall --neuron-name hue
```

[Uninstalling Manually]

To remove a resource, there's a need to delete the folder from the corresponding `resource_directory`.

```bash
cd /path/to/resource_folder
rm -rf <resource_name>
```

Example

```bash
cd /home/me/my_intelora_config/resources/neurons
rm -rf hue
```

[NOTE]
It should be noted that when deleting a resource folder, libraries that have been installed by the resource are not removed from the system. If a complete clean up is what the user desires, install.yml file of the resource must be opened, to see what have been installed and rollback manually each task.

For example, when installing the neuron hue, the python lib called phue has been installed. To perform a complete cleanup, you need to run "sudo pip uninstall phue".


[Update a resource]

To be able to update a resource, the user can either:
- uninstall the resource and then reinstall it back
- go into the resource directory and run a `git pull`
