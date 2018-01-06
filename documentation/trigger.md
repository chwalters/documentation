[Triggers]

With Intelora project, the user can set whatever Hotword he/she wants to wake it up. 


**Snowboy**

The use can create their own magic word by connecting to [Snowboy] -> (https://snowboy.kitt.ai/), and then download the trained model file.

Once downloaded, place the file in the personal config folder and configure snowboy in the [settings] -> (settings.md), following the table below

| parameter   | required | type   | default | choices         | comment 
|-------------|----------|--------|---------|-----------------|--------------------------------------------------------------------------------------------------
|
| pmdl_file   | TRUE     | string |         |                 | Path to the snowboy model file. The path can be absolute or relative to the brain file    |
| sensitivity | FALSE    | string | 0.5     | between 0 and 1 | Increasing the sensitivity value lead to better detection rate, but also higher false alarm rate |


If the user wants to keep "Intelora" as the name of their bot, we recommend to __enhance the existing Snowboy model for your language__.

We will update the following list with all Intelora model created by the community. If the model doesn't exist, please create one with the following syntax:
```
intelora-<language_code>
```

Example:

```
intelora-FR
intelora-EN
intelora-RU
intelora-DE
intelora-IT
```
Then, open an issue or create a pull request to add the model to the list bellow.

**List of available Snowboy Intelora model**

[IMPORTANT NOTE] Do not enhance a model in the wrong language. Check the pronunciation before recording the voice!

| Name                                                 | language | Pronounced   |
|------------------------------------------------------|----------|--------------|
| [intelora-FR](https://snowboy.kitt.ai/hotword/1363)  | French   | Ka-lio-pé    |
| [intelora-EN](https://snowboy.kitt.ai/hotword/2540)  | English  | kə-LIE-ə-pee |
| [intelora-RU](https://snowboy.kitt.ai/hotword/2964)  | Russian  | каллиопа     |
| [intelora-DE](https://snowboy.kitt.ai/hotword/4324)  | German   | Ka-lio-pe    |
| [intelora-IT](https://snowboy.kitt.ai/hotword/10650) | Italian  | Ka-lljo-pe   |
