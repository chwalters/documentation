[Neurons]

**-------**

Neuron is a plugin that executes a specific action. This is used to create a synapse. It is possible to append as many neurons as the user wants into a synapse. These are executed one by one when the input order is triggered.

If the user wants to install a community neuron, the [neuron list documentation] -> (neuron_list.md) can be checked for more information.

[Usage]

Neurons are declared in the `neurons` section of a synapse in the brain file.
The `neurons` section is a list, because it starts with a "-", which contains neuron module names

```yml
neurons:
    - neuron_1_name
    - neuron_2_name
    - another_neuron
```

Some neurons need parameters that can be passed as arguments following the syntax below:
```yml
neurons:
    - neuron_name:
        parameter1: "value1"
        parameter2: "value2"
```

[NOTE]
-> The parameters are indented with two spaces under the neuron's name and following the YAML syntax.
-> Intelora will try to load the neuron from your resources directory, then from core neuron packages.

To know the list of required parameters, the user can check the documentation of the neuron.
Full list of [available neuron] -> (neuron_list.md)

------

[Input values]

Neurons are necessary to have some **parameters** from the synapse declaration, for it to work. Those parameters, that are also called arguments, can be passed to the neuron in three (3) ways:

#from the neuron declaration
#from global variables
#from the captured order

>From the neuron declaration:

```yml
neurons:
    - neuron_name:
        parameter1: "value1"
        parameter2: "value2"
```

>From global variables: (cf: [settings.md] -> (settings.md))
```yml
  - name: "run-simple-sleep"
    signals:
      - order: "Wait for me "
    neurons:
      - sleep:
          seconds: {{variable}}
```

>From the captured order:
```yml
  - name: "say-hello"
    signals:
      - order: "say hello to {{ name }}"
    neurons:
      - say:
          message:
            - "Hello {{ name }}"
```

Here, the spoken value captured by the TTS engine will be passed as an argument to the neuron in every parameters that wants use it.

Example: 
With the synapse declaration above, if the user say "say hello to Bob". The parameter message is instantiated and all `{{ name }}` are replaced by "bob".

We suggest that the user should check the [signals documentation] -> (signals.md), for a complete understanding of how arguments in a neuron work.

[NOTE]
If a parameter of a neuron is waiting for a variable from the order and this variable haven't been found in the spoken order, then the neuron will not be launched.

------

[Output values]

There are some neurons which will return variables into a dictionary of value. Those values can be used to make a user's own Intelora answer through a template.
The objective of this is to let the user choose what he/she wants to make Intelora saying in its own language.

A template is a text file that contains **variables**, which get replaced with values when the template is evaluated by 
the [template engine](https://en.wikipedia.org/wiki/Jinja_(template_engine)), and **tags**, which control the logic of the template.

The template engine used in Intelora is [Jinja2](http://jinja.pocoo.org/docs/dev/).

An example of this, is when we look at the [documentation of the neuron systemedate] -> (../intelora/neurons/systemdate/README.md).
The neuron will return a dictionary of value like `minute`, `hours` and all other values about the current time on the system where Intelora is installed.

A simple that only use **variables**, template would be
```
It is {{ hours }} and {{ minutes }} minutes.
```

Placed in a complete synapse, it looks like the following:

```yml
- name: "time"
    signals:
      - order: "what time is it"
    neurons:
      - systemdate:
          say_template:
            - "It is {{ hours }} hours and {{ minutes }} minutes"
```

We use [variables] -> (http://jinja.pocoo.org/docs/dev/templates/#variables), from the neuron into our template file. 
Both variables will be interpreted by the template engine. 

So, what the user will hear is something like `It is 9 hours and 21 minutes`.

We can append some logic to a template with tags. 

Here is a simple example that comes with [a test tag] -> (http://jinja.pocoo.org/docs/dev/templates/#if), that will make Intelora change the pronounced sentence depending on the current time.
```
{% if hours > 8 %}
    It is late, isn't it?
{% else %}
    We still have time
{% endif %}
```

As this is multi-lines, we can put the content in a file and use a `file_template` instead of a `say_template` for more clarity.
```yml
- name: "time"
    signals:
      - order: "what time is it"
    neurons:
      - systemdate:
          file_template: /path/to/file/template.j2
```

**intelora_memory: Store in memory a variable from an order or generated from a neuron**

Intelora can store in a short term memory:
#output parameters from a neuron
#variable parameters captured from an order.

Stored parameters can then be used in other synapses during a future call.

[NOTE] It should be noted that this memory is not preserved after a restart of Intelora.

**Store parameters generated by a neuron**

Syntax with output parameters from a neuron

```yml
- name: "synapse-name"
  signals:
    - order: "my order"
  neurons:
    - neuron_name:
        intelora_memory:
          key_name_in_memory: "{{ output_variable_from_neuron }}"
          other_key_name_in_memory: "{{ other_output_variable_from_neuron }}"
```

Syntax to reuse memorized parameters in another synapse
```yml
- name: "synapse-name"
  signals:
    - order: "an order"
  neurons:
    - neuron_name:
        parameter1: "{{ intelora_memory['key_name_in_memory'] }}"
        parameter2: "{{ intelora_memory['other_key_name_in_memory'] }}"        
```

[NOTE] The key name need to be placed into simple quotes 


Example with a core neuron like `systemdate`:

```yml
- name: "synapse-name"
  signals:
    - order: "my order"
  neurons:
    - systemdate:
        say_template:
          - "It' {{ hours }} hours and {{ minutes }} minutes"         
        intelora_memory:
          hours_when_asked: "{{ hours }}"
          minutes_when_asked: "{{ minutes }}"
```

Here, the `systemdate` neuron generates variables that have been passed to the template like described in the previous section and to the memory of Intelora.

Those parameters can now be used in a next call

```yml
- name: "synapse-name-2"
  signals:
    - order: "a what time I've asked the time?"
  neurons:
    - say:
        message:
          - "at {{ intelora_memory['hours_when_asked']}} hours and {{ intelora_memory['minutes_when_asked']}} minutes"
```

As it's based on a template, the value can be modified by adding a string

```yml
intelora_memory:
  my_saved_key: "{{ neuron_parameter_name }} with a word"
```

Multiple parameters can be used and concatenated in the same memorized key

```yml
intelora_memory:
  my_saved_key: "{{ neuron_parameter_name1 }} and {{ neuron_parameter_name2 }}"
``` 

**Store parameters captured from orders**

Syntax
```yml
- name: "synapse-name"
  signals:
    - order: "my order with {{ variable }}"
  neurons:
    - neuron_name:
        intelora_memory:
          key_name_in_memory: "{{ variable }}"         
```

The syntax to reuse memorized parameters in another synapse is just the same as the one used with neuron parameters

```yml
- name: "synapse-name"
  signals:
    - order: "an order"
  neurons:
    - neuron_name:
        parameter1: {{ intelora_memory['key_name_in_memory']}}               
```

Example
```yml
- name: "synapse-id"
  signals:
    - order: "say hello to {{ name }}"
  neurons:
    - say:
        message:
          - "Hello {{ name }}"
        intelora_memory:
          friend: "{{ name }}"
```

In here, the variable "name" has been used directly into the template and also saved in memory behind the key "friend".

The value can now be used in a next call like the following:

```yml
- name: "synapse-id"
  signals:
    - order: "what is the name of my friend?"
  neurons:
    - say:
        message:
          - "It's {{ intelora_memory['friend'] }} 
```

Here is another example, brain which uses the `neurotimer` neuron. 

In this situation, you want to be reminded to do something.

[YOU:] remind me to call mom in 15 minutes
[Intelora:] I'll notify you in 15 minutes
15 minutes later..
[Intelora:] You asked me to remind you to call mom 15 minutes ago

```yml
- name: "remember-synapse"
  signals:
    - order: "remind me to {{ remember }} in {{ time }} minutes"
  neurons:
    - neurotimer:
        seconds: "{{ time }}"
        synapse: "remember-todo"
        intelora_memory:
          remember: "{{ remember }}"
          seconds: "{{ time }}"
    - say:
        message:
          - "I'll remind you in {{ time }} minutes"

- name: "remember-todo"
  signals: {}
  neurons:
    - say:
        message:
          - "You asked me to remind you to {{ intelora_memory['remember'] }} {{ intelora_memory['time'] }} minutes ago"
```


[Overridable parameters]

For each neuron, it is possible to override some parameters, for the purpose of using a specific configuration instead of the default one set in [settings.yml] -> (settings.md) file.

#TTS

The user can override the default TTS configuration like the TTS engine to use and its parameters like the language, api key or the cache.

By default Intelora uses a cache to save a generated audio from a TTS engine.
This cache is useful to manage and organize sentences that are not suppose to be changed very often. 

Here's an example, the following sentence will not change in time, so it's more optimized to generate it once and to keep it in cash:

```yml
- say:
    message:
      - "Hello, sir"
```

There are some cases, especially when the neuron is based on a template, the generated audio will change on each new call of the neuron and so the usage of a cache is not necessary. 

Given as a best example of a case like this is the `systemdate` neuron. 
As the time changes every minute, the generated audio will change too and so, saving the generated audio in the cache is useless. In this case, you can possibly override the cache usage for this neuron:

Here are the default TTS settings in my `settings.yml` file

```yml
default_text_to_speech: "pico2wave"

text_to_speech:
  - pico2wave:
      language: "fr-FR"
      cache: True
```

And this is how to override only the cache parameter:

```yml
- systemdate:
    say_template:
      - "It's {{ hours }} hour and {{ minutes }} minute"
    tts:
      pico2wave:
        cache: False
```

It is possible to override all parameter for each neurons like in the example below
```yml
- name: "say-something-in-spanish"
    signals:
      - order: "Say hello in spanish"
    neurons:
      - say:
          message:
            - "Buenos dias"
          tts:             
            pico2wave:
              language: "es-ES"
              cache: False
```

[NOTE]
The TTS should be configured with its required parameters in the settings.yml file to be overridden. Check the [TTS documentation] -> (tts.md).
