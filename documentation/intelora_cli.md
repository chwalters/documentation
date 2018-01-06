[Intelora Command-line interface]

**-------**

[SYNOPSIS]

This is the syntax used to run Intelora from command line
```bash
intelora command --option <argument>
```

For example, to start Intelora we simply use
```bash
intelora start
```

[NOTE] Do not use the CLI as root user or with sudo. Intelora must run with standard user privileges.

[ARGUMENTS]

#start
Start Intelora main program

Example of use
```bash
intelora start
```

To stop the processes of Intelora, just press "Ctrl-C" on your keyboard.

#gui

Launch the Intelora shell Graphical User Interface.
 
This GUI allows you to test your [STT](stt.md) and [TTS](tts.md) that you have configured in [settings.yml] -> (default_settings.md) file of Intelora.

Example of use
```bash
intelora gui
```

#install
Install a community module. The user must set an install type option. 
Currently, the only available option is `--git-url`.

Syntax
```bash
intelora install --git-url <url>
```

Example of use
```bash
intelora install --git-url https://github.com/intelora-project/intelora_neuron_wikipedia.git
```

#OPTIONS

Commands can be completed by the following options:

**-v or --version**
Display the current installed version of Intelora.

Example of use
```bash
intelora --version
```

```bash
intelora -v
```

**-run-synapse SYNAPSE_NAME**

Execute a specific synapse from the brain file.

Example of use
```bash
intelora start --run-synapse "say-hello"
```

**--run-order "Your Order"--**

Execute a specific order from command line.

Example of use
```bash
intelora start --run-order "hello"
```

**--brain-file BRAIN_FILE**

Replace the default brain file from the root of the project folder by a custom one.

[Important note:] The path must be absolute. The absolute path contains the root directory and all other subdirectories in which a file or folder is contained. 

Example of use
```bash
intelora start --brain-file /home/me/my_other_brain.yml
```

You can combine the options together like, for example:
```bash
intelora start --run-synapse "say-hello" --brain-file /home/me/my_other_brain.yml
```

**--debug**

This will show the debug output in the console

Example of use
```bash
intelora start --debug
```

**--git-url**

Used by the `install` argument to specify the URL of a git repository of the module to install.

