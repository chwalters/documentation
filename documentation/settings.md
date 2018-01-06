[Intelora settings]

**-------**

This documentation will explain the main configuration of Intelora placed in the (settings.md) file.

[Triggers configuration]

**default_trigger**

The trigger is the module that detects the hotword, and wakes up Intelora.
Common usage of hotword include Alexa on Amazon Echo, OK Google on some Android devices and Hey Siri on iPhones.

Specifying the name of the trigger module that will be used:

```yml
default_trigger: "trigger_name"
```

Available triggers for Intelora are:
- snowboy

**triggers**

The hotword (also called a "wake word" or "trigger word") detector is the engine in charge of waking up Intelora. Each Trigger has it own configuration. 

This configuration is passed as argument following the syntax below:

```yml
triggers:
  - trigger_name:
      parameter_name: "value"
```

Example:
the default Snowboy trigger configuration is:

```yml
triggers:
  - snowboy:
      pmdl_file: "trigger/snowboy/resources/model.pmdl"
```

See the complete list of [available triggers] -> (trigger.md).

[Players configuration]

**default_player**

The Player is the module managing the sound in Intelora.

The user can specify the name of the player module they want to use.

```yml
default_player: "player_name"
```
Example:

```yml
default_player: "mplayer"
```

**players**

Each Players has it own configuration. 

This configuration is passed as argument following the syntax below:

```yml
players:
  - player_name:
      parameter_name: "value"
```

Example:

```yml
players:
  - mplayer: {}
  - pyalsaaudio:
     device: "default"
     convert_to_wav: True
  - pyaudioplayer:
     convert_to_wav: True
  - sounddeviceplayer:
     convert_to_wav: True
```

See the complete list of [available players] -> (player_list.md)

When no parameters are required set an empty object:

```yml
players:
  - mplayer: {}
```

Sometimes, parameters will be necessary to use an engine. See the [complete list] -> (player_list.md), to know which parameter are required.
Core players are already packaged with the installation of Intelora and can be used out of the box. 

[NOTE]
Most cloud based TTS generate a file in MP3 format. Some players are not able to read this format and then a conversion to wav is needed.


[Speech to text configuration]

**default_speech_to_text**

A Speech To Text(STT) is an engine used to translate what the user say, into a text that can be processed by Intelora core.
By default, Intelora uses google STT engine.

The following syntax is used to provide the engine name:

```yml
default_speech_to_text: "stt_name"
```

Example:

```yml
default_speech_to_text: "google"
```

Get the full list of [SST engine] -> (stt.md).

**speech_to_text**

Each STT has it own configuration. 
This configuration is passed as argument as shown below:

```yml
speech_to_text:
  - stt_name:
      parameter_name: "value"
```

Example:

```yml
speech_to_text:
  - google:
      language: "fr-FR"
  - bing
```

Some arguments are required, some others are optional. Refer to the [STT documentation] -> (stt.md), to know available parameters for each supported STT.

**recognition_options**

Represents a collection of speech recognition settings and functionality:

```yml
recognition_options:
  option_name: option_value
  option_name2: option_value2
```

Example:

```yml
recognition_options:
  energy_threshold: 3000
```

**energy_threshold**

Represents the energy level threshold for sounds. By default, set to **4000**.

Values below this threshold are considered silence, and values above this threshold are considered speech.
This is adjusted automatically if dynamic thresholds are enabled with `adjust_for_ambient_noise_second` parameter.

This threshold is associated with the perceived loudness of the sound, but it is a nonlinear relationship. 
The actual energy threshold will be needed depends on the user's microphone sensitivity or audio data. 
Typical values for a silent room are 0 to 100, and typical values for speaking are between 150 and 3500. 
Ambient (non-speaking) noise has a significant impact on what values will work best.

If the user is having trouble with the recognizer trying to recognize words even when not speaking, tweaking this to a higher value can be tried. 
If the user is having trouble with the recognizer not recognizing the words when speaking, tweaking this to a lower value can be tried. 


For example, a sensitive microphone or microphones in louder rooms might have a ambient energy level of up to 4000.
```yml
recognition_options:
  energy_threshold: 4000
```

[NOTE] The default value is 4000 if not set.

**adjust_for_ambient_noise_second**

If defined, will adjusts the energy threshold dynamically by capturing the current ambient noise of the room during the number of second set in the parameter.
When set, the `energy_threshold` parameter is overridden by the returned value of the noise calibration.
This value should be at least 0.5 in order to get a representative sample of the ambient noise.

```yml
recognition_options:
  adjust_for_ambient_noise_second: 1
```

[NOTE] The number of second here represents the time between Intelora's awakening and the moment when the user can give the order.

[Text to speech configuration]

**default_text_to_speech**

A Text To Speech is an engine used to translate written text into a speech, into an audio stream.
By default, Intelora use [Pico2wave] TTS engine.

The following syntax is used to provide the TTS engine name:

```yml
default_text_to_speech: "tts_name"
```

Example:

```yml
default_text_to_speech: "pico2wave"
```

The full list of [TTS engine] -> (tts.md).

**text_to_speech**

Each TTS has it own configuration. 
This configuration is passed as argument following the syntax below:

```yml
text_to_speech:
  - tts_name:
      parameter_name: "value"
```

Example:

```yml
text_to_speech:
  - pico2wave:
      language: "fr-FR"
  - googletts:
      language: "fr"
```

Some arguments are required, some other optional, refer to [TTS documentation] -> (tts.md), to know available parameters for each supported TTS.

[Wake up answers configuration]

**random_wake_up_answers**

When Intelora detects your trigger/hotword/magic word, it lets you know that it's operational and now waiting for order. It's done by answering randomly.

One of the sentences provided in the variable, random_wake_up_answers.

This variable must contain a list of strings as shown below:

```yml
random_wake_up_answers:
  - "You sentence"
  - "Another sentence"
```

Example:

```yml
random_wake_up_answers:
  - "Yes sir?"
  - "I'm listening"
  - "Sir?"
  - "What can I do for you?"
  - "Listening"
  - "Yes?"
```

**random_wake_up_sounds**

The user can play a sound when Intelora detects the hotword/trigger instead of saying something from the `random_wake_up_answers`.

The user can place here a list of full paths of the sound files you want to use. Otherwise, you can use some default sounds provided by Intelora which you can find in `/usr/lib/intelora/sounds`.

By default, two files are provided: ding.wav and dong.wav. In all cases, the file must be in `.wav` or `.mp3` format. If more than on file is present in the list, Intelora will select one randomly at each wake up.

```yml
random_wake_up_sounds:
  - "local_file_in_sounds_folder.wav"
  - "/my/personal/full/path/my_file.mp3"
```

Example:

```yml
random_wake_up_sounds:
  - "ding.wav"
  - "dong.wav"
  - "/my/personal/full/path/my_file.mp3"
```

[NOTE] If you want to use a wake up sound instead of a wake up answer you must comment out the `random_wake_up_answers` section.

Example: `# random_wake_up_answers:`


**On ready notification**

This section is used to notify the user when Intelora is waiting for a trigger detection by playing a sound or speak a sentence out loud

**play_on_ready_notification**

This parameter defines if the user plays the on ready notification:

#`always`: every time Intelora is ready to be awaken
#`never`: never play a sound or sentences when intelora is ready
#`once`: at the first start of Intelora

Example:

```yml
play_on_ready_notification: always
```
**on_ready_answers**

The on ready notification can be a sentence. Place here a sentence or a list of sentence. If you set a list, one sentence will be picked up randomly

Example:

```yml
on_ready_answers:
  - "I'm ready"
  - "Waiting for order"
```

**on_ready_sounds**

The user can play a sound instead of a sentence.
Remove the `on_ready_answers` parameters by commenting it out and use this one instead.
Place here the path of the sound file. 

[NOTE] Files must be .wav or .mp3 format.

Example:

```yml
on_ready_sounds:
  - "ding.wav"
  - "dong.wav"
  - "/my/personal/full/path/my_file.mp3"
```

[NOTE] If the user wants to use an on ready sound, the user must comment out the `random_on_ready_answers` section.
Example: `# random_on_ready_answers:`


[Rest API]

A Rest API can be activated in order to:
- List synapses
- Get synapse's detail
- Run a synapse

For the complete API reference, see the [REST API documentation] -> (rest_api.md)

Settings examples:

```yml
rest_api:
  active: True
  port: 5000
  password_protected: True
  login: admin
  password: secret
  allowed_cors_origin: "*"
```

**active**
To enable the rest api server.

**port**
The listening port of the web server. Must be an integer in range 1024-65535.

**password_protected**
If `True`, the whole api will be password protected.

**Login**
Login used by the basic HTTP authentication. Must be provided if `password_protected` is `True`.

**Password**
Password used by the basic HTTP authentication. Must be provided if `password_protected` is `True`.

**Cors request**
If the user wants to allow request from external application, enabling the CORS requests settings by defining authorized origins is needed.
To do so, just indicated the origins that are allowed to leverage the API. The authorize values are:

False to forbid CORS request:

```yml
allowed_cors_origin: False
```

or either a string or a list:
```yml
allowed_cors_origin: "*"
```
(in case of "*", all origins are accepted).
or
```yml
allowed_cors_origin:
  - 'http://mydomain.com/*'
  - 'http://localhost:4200/*'
```

[NOTE] It should be noted that an origin is composed of the scheme (http(s)), the port (eg: 80, 4200,…) and the domain (mydomain.com, localhost).

[Default synapse]

Run a default [synapse] -> (brain.md) when Intelora can't find the order in any synapse or if the SST engine haven't understood the order.

```yml
default_synapse: "synapse-name"
```

Example:

```yml
default_synapse: "Default-response"
```

[Resources directory]

The resources directory is the path where Intelora will try to load community modules like Neurons, STTs or TTSs.
Setting a valid path is required if the user wants to install community neuron. The path can be relative or absolute.

```yml
resource_directory:
  resource_name: "path"
```

Example:

```yml
resource_directory:
  neuron: "resources/neurons"
  stt: "resources/stt"
  tts: "resources/tts"
  trigger: "/full/path/to/trigger"
```


[Global Variables]

The Global Variables paths list where to load the global variables.
Those variables can be reused in neuron parameters within double brackets.

Example:

```yml
var_files:
  - variables.yml
  - variables2.yml
```
[NOTE] If a variable is defined in different files, the last file defines the value.

In the files the variables are defined by key/value:

```yml
variable: 60
baseURL: "http://blabla.com/"
password: "secret"
```

And use variables in your neurons:

```yml
  - name: "run-simple-sleep"
    signals:
      - order: "Wait for me "
    neurons:
      - uri:
          url: "{{baseURL}}get/1"        
          user: "admin"
          password: "{{password}}"
```

[NOTE] Because YAML format does not allow double braces not surrounded by quotes: you must use the variable between double quotes. 

A global variable can be a dictionary. 

Example:

```yml
contacts:
  nico: "1234"
  tibo: "5678"
```

And a synapse that use this dict:
```yml
- name: "test-var"
  signals:
    - order: "give me the number of {{ contact_to_search }}"
  neurons:
    - say:
        message:
        - "the number is {{ contacts[contact_to_search] }}"
```

[Raspberry LED and mute button]

LEDs connected to GPIO port of the Raspberry can be used to know current status of Intelora.
A button can also be added in order to pause the trigger process. Intelora does not listen for the hotword anymore when pressed.

A Dictionary called `rpi` can be declared which contains pin number to use following the mapping bellow

| Value name        | Description                                                                                                |
|-------------------|------------------------------------------------------------------------------------------------------------|
| pin_mute_button   | Pin connected to a mute button. When pressed the trigger process of intelora is paused                     |
| pin_led_started   | Pin switched to "on" when Intelora is running                                                              |
| pin_led_muted     | Pin switched to "on" when the mute button is pressed                                                       |
| pin_led_talking   | Pin switched to "on" when Intelora is talking                                                              |
| pin_led_listening | Pin switched to "on" when Intelora is readu to listen an order after a trigger detection ("Say something") |

**Example config**
```yml
rpi:
  pin_mute_button: 6
  pin_led_started: 5
  pin_led_muted: 17
  pin_led_talking: 27
  pin_led_listening: 22
```

You can also define a couple led instead of all if you don't use them
```yml
rpi:
  pin_mute_button: 6
  pin_led_started: 5
#  pin_led_muted: 17
#  pin_led_talking: 27
#  pin_led_listening: 22
```

**Example circuit**

You will be using one of the ‘ground’ (GND) pins to act like the ‘negative’ or 0 volt ends of a battery. 
The ‘positive’ end of the battery will be provided by a GPIO pin.

<p align="center">
    <img style="width: 200px;" src="../images/led_intelora_circuit.png">
</p>


[NOTE] It's a must to ALWAYS use resistors to connect LEDs up to the GPIO pins of the Raspberry Pi. 
The Raspberry Pi can only supply a small current (about 60mA). T
he LEDs will want to draw more, and if allowed to they will burn out the Raspberry Pi. 
Therefore putting the resistors in the circuit will ensure that only this small current will flow and the Pi will not be damaged.


>Configure the brain of Intelora:
Now the settings are dealt, start creating the [brain] -> (brain.md) of the assistant.
