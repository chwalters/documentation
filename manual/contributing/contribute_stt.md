[Contributing: Create a STT]

**-------**

[STT](../stt.md) are independent projects so they can be developed under a github project. 
Anyone can clone them and place them under the STT repository and reuse them or directly [install](../stt.md) them

Creating a new STT must follow some rules:

**Repository Structure**

-> The STT repository name is in __lowercase__.
-> Under the STT repository, the STT has a __README.md file__ describing the STT following this structure. 
Get a [template here] -> (stt_template.md):

    - STT name:
    - Installation:     The CLI command used to install the STT
    - Synopsis:         Description of the STT
    - Options:          A table of the incoming parameters managed by the STT.
    - Notes:            Something which needs to be add.
    - Licence:          The licence you want to use

-> Under the STT repository, a [dna.yml file](dna.md) must be added that contains information about the STT. type = "stt"
-> Under the STT repository, a [install.yml file](installation_file.md) must be added that contains the installation process.


**Code**
-> Under the STT repository, the STT file name .py is also in __lowercase__.
-> The STT must be coded in __Python 2.7__.
-> Under the STT repository, include the __init__.py file which contains: *from stt import STT* (/!\ respect the Case)
-> Inside the STT file, the STT Class name is in __uppercase__.
-> The STT __inherits from the SpeechRecognition__ coming from the Utils file in the STT package.

    ```python
    from intelora.stt.Utils import SpeechRecognition
    class Google(SpeechRecognition):
    ```

-> The STT has a constructor __init__ which is the entry point.
The constructor has an incoming callback to call once we get the text.
The constructor has a __**kwargs argument__ which is corresponding to the Dict of incoming variables:values defined either in the settings file.
-> The STT must init itself first.
-> Attach the incoming callback to the self.main_controller_callback attribute. This callback come from the main controller and will receive the text at the end of the process.
-> Obtain audio from the microphone in the constructor. (Note : we mostly use the [speech_recognition library](https://pypi.python.org/pypi/SpeechRecognition/))
-> Set the callback method into the mother class with `self.set_callback(self.google_callback)`. This callback will process the audio into a text.
-> Use self.start_processing() from the mother class to get an audio from the microphone or read the audio file path if provided. The mother class will give the audio stream to the callback you've set before.
-> The callback method must implement two arguments: recognizer and audio. The audio argument contains the stream caught by the microphone or read from an audio file path
-> Do magic stuff with the audio in order to get a string that contains the translated text
-> Once you get the text, let give it to the main_controller_callback method received in the constructor by calling it with the text string as argument `self.main_controller_callback(audio_to_text)`

    ```python
    def __init__(self, callback=None, **kwargs):
        # give the audio file path to process directly to the mother class if exist
        SpeechRecognition.__init__(self, kwargs.get('audio_file_path', None))
        # here is the main controller callback. We will return the text at the end of the process
        self.main_controller_callback = callback
        
        self.argument_from_settings = kwargs.get('argument_from_settings', None)
        
        #  give our callback   
        self.set_callback(self.my_callback)
        # start processing, record a sample from the microphone if no audio file path provided, else read the file
        self.start_processing()
        
        def my_callback((self, recognizer, audio):
            # ---------------------------------------------
            # do amazing code to translate the audio stream into text
            # 'audio' contain stream caught by the microphone
            # ---------------------------------------------
            
            # at the end of the process, send the text into the received callback method
            self.main_controller_callback(audio_to_text)
    ```



**Code example**

Example of STT structure:

```
mystt/
├── __init__.py
├── mystt.py
├── dna.yml
├── install.yml
└── README.md
```

Example of STT code:
- [Google STT](../../intelora/stt/google/README.md)
