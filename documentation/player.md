[Players]

**-------**
                                                                                                                                       
The Player is the library or software that is used to make Intelora talk. With Intelora project, a user can set any sound player they want to use.

[Player Settings]

The user can define the player they want to use by default in [setting.yml] -> (settings.md) file.

```yml
default_player: "player_name"
```

Example
```yml
default_player: "mplayer"
```

Then, still in the [setting.yml] -> (settings.md) file, each one of the player must set up its configuration following the 'players' tag :

```yml
players:
   - player1:
      player1parameter1: "value option1"
      player1parameter2: "value option2"
   - player2:
      player2parameter1: "value option1"
```

Example
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

[NOTE] There are times that, parameters will be necessary to use an engine. 
Click on a Player engine link in the `Current CORE Available Players` section to know which parameter are required.

[NOTE] A player which does not ask for input parameters need to be declared as an empty dict. 

Example: ```- player_name: {}```

**Current CORE Available Players**

Core players are already packaged with the installation of Intelora and it can be used out of the box. The [complete list] is here ->(player_list.md).

**Full Example**

In the (settings.yml) file:

```yml
default_player: "mplayer"

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
