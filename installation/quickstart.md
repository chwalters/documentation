[Intelora quick start]

**-------**

Intelora needs two files to works, a (`settings.yml`) and a (`brain.yml`). 
As files are written on YAML syntax, we strongly recommend you to use an editor(IDE) like [VS Code] (https://code.visualstudio.com/) or [Atom](https://atom.io/). 

If the user is using Intelora from a Rpi, the idea would be to configure the assistant from the main computer with an IDE and then push the config folder into the Rpi.

We made starter kits that only needs to be cloned, placed into the Rpi and launched.
- [French starter kit](https://github.com/intelora-project/intelora_starter_fr)
- [English starter kit](https://github.com/intelora-project/intelora_starter_en)
- [German starter kit](https://github.com/intelora-project/intelora_starter_de)

Those repositories provides a structure to start playing and learning basics of Intelora.
Download the starter kit of your choice and open the folder with your IDE. 

When the user started intelora using the CLI (`intelora start`), the program will try to load the `settings.yml` and `brain.yml` in the following order:
- From current folder, Example: `/home/pi/my_intelora/settings.yml`
- From `/etc/intelora/settings.yml`
- From the default `settings.yml`. Take a look into the default [`settings.yml`] -> (../intelora/settings.yml) file which is located in the root of the project tree.

This is a common tree of a Intelora configuration folder:

```
intelora_config/
├── brains
│   └── included_brain.yml
├── brain.yml
├── files
│   └── intelora-EN-13samples.pmdl
└── settings.yml
```

-> Open the main brain file. What the user will see there, are some included sub brains file. 

```yml
- includes:
    - brains/say.yml
```

If the user opened the (`say.yml`) file from the brains folder, a basic synapse that uses the [neuron](../neurons.md) "[Say](../../intelora/neurons/say)" and make Intelora speaks out loud "Hello sir" when the user says "hello" will be seen.

-> Move into the folder and then start Intelora:

```bash
cd /path/to/the/starter_kit
intelora start
```
[NOTE] Do not start Intelora as root user or with sudo

Intelora will load settings and brain, the output should looks the following:

```bash
Starting event manager
Events loaded
Starting Intelora
Press Ctrl+C for stopping
Starting REST API Listening port: 5000
Waiting for trigger detection
```

-> Then speak the hotword out loud to wake up Intelora (with the right pronunciation depending on the user's starter kit. "Intelora" in English).

-> If the trigger is successfully raised, the user will see "say something" into the console. 

```bash
2016-12-05 20:54:21,950 :: INFO :: Keyword 1 detected at time: 2016-12-05 20:54:21
Say something!
```

-> Then, the user can say "hello" and listen the Intelora response.

```bash
Say something!
Google Speech Recognition thinks you said hello
Order matched in the brain. Running synapse "say-hello"
Waiting for trigger detection
```

Then, that's it. The user can go and explore customizing their own assistant.

Recommended to do:

- See what the user can customize in [settings] -> (../settings.md) like changing the hotword, the STT or TTS engine.
- Create the [brain] -> (../brain.md)
- See the list of [available neurons] -> (../neuron_list.md)
