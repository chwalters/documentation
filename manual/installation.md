[Intelora installation]

**-------**

[Pre-requisites]

Follow the right link below to install requirements depending on the target environment:
- [Raspbian (Raspberry Pi 2 & 3)](installation/raspbian.md)
- [Ubuntu 14.04](installation/ubuntu_14.04.md)
- [Ubuntu 16.04](installation/ubuntu_16.04.md)
- [Debian Jessie/Strech](installation/debian.md)

[Installation]

#Method 1 - User install using the PIP package

Installation of intelora on system using Pypi:
```bash
sudo pip install intelora
```

#Method 2 - Manual setup using sources

Installation of intelora by cloning the project:
```bash
git clone https://github.com/intelora-project/intelora.git
cd intelora
```

Install the project:
```bash
sudo python setup.py install
```

#Method 3 - Developer install using Virtualenv

Installation of intelora by the `python-virtualenv` package:
```bash
sudo apt-get install python-virtualenv
```

Clone the project:
```bash
git clone https://github.com/intelora-project/intelora.git
cd intelora
```

Generate a local python environment:
```bash
virtualenv venv
```

Install the project using the local environment:
```bash
venv/bin/pip install --editable .
```

Activate the local environment:
```bash
source venv/bin/activate
```

#Method 4 - Developer, dependencies install only

Installation of intelora by cloning the project:
```bash
git clone https://github.com/intelora-project/intelora.git
cd intelora
```

Install the python dependencies directly:
```bash
sudo pip install -r install/files/python_requirements.txt
```

#Test the environment

This can be tried to make sure that the user can record their voice. 

-> Just run the following command to capture audio input from the microphone:
```bash
rec test.wav
```
-> Press CTRL-C after capturing a sample of your voice.

-> Then play the recorded audio file
```bash
mplayer test.wav
```

-> Then, the installation is now complete. 

Go to the [quickstart documentation] -> (installation/quickstart.md) to learn how to use Intelora.

[Get a starter configuration]

There are some starter configuration that can only be downloaded and it can be started. 
Those repositories will provide a basic structure when the user starts to explore Intelora. 
It is highly suggested that the user clone one of them and then go to the next section.

- [French starter config](https://github.com/intelora-project/intelora_starter_fr)
- [English starter config](https://github.com/intelora-project/intelora_starter_en)
- [German starter config](https://github.com/intelora-project/intelora_starter_de)
- [Czech starter config](https://github.com/intelora-project/intelora_starter_cs)
- [Italian starter config](https://github.com/intelora-project/intelora_starter_it)


[Start Intelora automatically after a reboot]

If the user wants to start Intelora automatically, the script should be placed below in `/etc/systemd/system/intelora.service`.
Update the path to your folder where the user placed the `brain.yml` and `settings.yml` file.

```bash
[Unit]
Description=Intelora

[Service]
WorkingDirectory=/path/to/intelora_brain_folder

Environment='STDOUT=/var/log/intelora.log'
Environment='STDERR=/var/log/intelora.err.log'
ExecStart=/bin/bash -c "/usr/local/bin/intelora start > ${STDOUT} 2> ${STDERR}"
User=%i

[Install]
WantedBy=multi-user.target
```

Then, reload systemctl, start the service and enable it at startup
```bash
sudo systemctl daemon-reload
sudo systemctl start intelora
sudo systemctl enable intelora
```
>Explore Intelora: 
After everything is done, start exploring Intelora. 
First, check the [default settings] -> (settings.md).
