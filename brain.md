[BRAIN]

**-------**

The Brain is the major and main component of Intelora. 

It is where the configuration of your own Intelligent Personal Assistant takes place. In here, functionalities can be determined as well as its behavior. The Brain is made up of Synapses.

The Synapes is the one who is capable of linking the input and output processes.

An input action, is referred to as a [signal] (signals.md)

A [signal] can be the following:

#an order: Whereas it is something that has been spoke out loud by the user.
#an event: A date or a frequency (Example: repeat each morning at 8:30)
#a mqtt message: A message received on a MQTT topic.
#a signal made the community
#No signal: Then the synapse can be only called from another synapse or by the API.


An output action, is referred to as a [neuron] (neurons.md)

A [neuron] is a module or plugin where some actions are performed, such as simply talking, running a script, running a command or even calling a web service.


The Brain is expressed in YAML (Yet Another Markup Language) format and has a minimum of syntax, which intentionally tries not be a programming language or script,  but it rather implicates as a model of a configuration or a process.

Intelora will look for the brain in the order given below:
- It starts from the user's current folder.
Example, `/home/pi/my_intelora/brain.yml`

- From `/etc/intelora/brain.yml`

- From the default, `brain.yml`. You can take a look into the default [`brain.yml`](../intelora/brain.yml) file which is located in the root of the project tree.

Here's a basic synapse in the Brain:

```yml
---
  - name: "Say-hello"
    signals:
      - order: "say hello"
    neurons:
      - say:
          message: "Hello, sir"    
```

The parts were illustrated by section to be fully understood by the user how the file is made and every part means.

The file starts with: `---` 
-> This is a major requirement for YAML to interpret the file as a proper document.

Items that begin with a ```-``` are considered as [list items]. 
Items have the format of ```key: value``` where value can be a simple string or a sequence of other items.

At the top level, there's the "name" tag. This is what we call, [unique identifier] of the synapse. It must be a unique word with the only accepted values, which are alphanumerics and dash. ([a - zA - Z0 - 9\-])

```yml
- name: "Say-hello"
```

The first part, called [signals] is the list of input actions. It works just as like the neurons. The user must place here at least one action.

In the following example, one signal was only used, which is we call an [order].

See the complete list of [available signals] in (signals.md).

```yml
signals:
  - order: "say-hello"
```

The user can append as much orders as they want for the signals. Orders can be appended even if they really do not mean the same thing. (For example, order "say hello" and order "adventure" or whatever), as long as they are in the same synapse, they will trigger the same action defined in neurons. 

It should be noted that it's not necessary for the users to put the exact sentence in the [order]. Intelora uses matching, in which the user can pronounce the sentence that contains the [order] and it will launch an attached task anyway.
 
In this example, the task attached to order "say hello" will be launched even if you say either of the following:

- "say hello Intelora"
- "Intelora, say hello"
- "I want you to say hello"
- "i say goodbye you say hello"
- "whatever I say as long it contains say hello"

To check if the user's order will be triggered by Intelora, we suggest to [use the GUI] (intelora cli.md) for testing your STT engine.

[NOTE]

Pay attention in defining the orders as precise as possible. As Intelora is based on matching, if the user defined the orders in different synapses similarly, Intelora risks to trigger more actions than the user is expecting.

As an example, if two different synapses was defined as shown below:

```yml
- name: "Say-hello"
  signals:
    - order: "say hello"
```
and
 
```yml
- name: "Say-something"
  signals:
    - order: "say"
```

When the user will pronounce "say hello", it will trigger both synapses. 

---

[Neurons declaration]

Neurons are the modules in which when the input action is triggered, it will be executed. 
Defining as many neurons as you want to the same input action, is possible. (for example: say somethning, then do something etc...) 
This declaration contains a list (because it starts with a "-") of neurons.

```yml
neurons:
    - neuron_1_name
    - neuron_2_name
    - another_neuron
```

The sequence execution of neurons is defined by the order in which they are properly listed within the neurons declaration.

There are some neurons that need parameters that can be passed as arguments, following the syntax below:
```yml
neurons:
    - neuron_name:
        parameter1: "value1"
        parameter2: "value2"
```
It should be noted that parameters are indented with one tabulation below the neuron's name because it is a YAML syntax requirement.

In the example, the neuron called "say" will make Intelora speak out loud the sentence in parameter **message**.
See the complete list of [available neurons] in (neuron_list.md).

[Managing Synapses]

Intelora also provides a REST API to handle the synapses (get the list, get one, run one). See the [rest api documentation] in (rest_api.md), for more information.


[Splitting the brain]

In intelora, there's a way where you can simply sort your actions in different files for better visibility. You can split the main brain file into multiple ones.

To do that, you will use the import statement in the entry brain.yml file with the following syntax:

```yml
  - includes:
      - path/to/sub_brain.yml
      - path/to/another_sub_brain.yml
```

Example:
```yml
  - includes:
      - brains/rolling_shutter_commands.yml
      - brains/find_my_phone.yml
```

[NOTE] 
-> User can only use the `include` statement in the main brain file. 
-> the include(s) statement must start with a `-`.


[The default Synapse]

User can create a default synapse in case none of them are matching when an order is given.

[NOTE] 
The default synapse is optional.
It is defined in the settings.yml cf: [Setting] (settings.md).

**-------**

>Start Intelora: 
Take another step and look into the (intelora_cli.md) where you can find the [CLI documentation], to learn how to start intelora.

>For more information:
- About [neuron] -> (neurons.md)
- About [signal] -> (signals.md)

