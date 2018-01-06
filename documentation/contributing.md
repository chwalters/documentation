[Contributing]

**-------**

[Core]

The community can contribute to the Core of Intelora by providing some new features.

#How to contribute

1. Fork it!
2. Checkout the dev branch `git checkout dev`
3. Create your feature branch: `git checkout -b my-new-feature`
4. Commit your changes: `git commit -am 'Add some feature'`
5. Push to the branch: `git push origin my-new-feature`
6. Submit a pull request in the **dev** branch

[Community module (Neuron, STT, TTS)]

Intelora comes with a community [installation command] -> (intelora_cli.md). 
A user can create a module in their own git repo that will be available to all Intelora users.

Below are the dedicated documentation depending on the type of module you want to code.
- create a [community neuron](contributing/contribute_neuron.md)
- create a [community STT](contributing/contribute_stt.md)
- create a [community TTS](contributing/contribute_tts.md)


[Constraints]

1. Respect [PEP 257](https://www.python.org/dev/peps/pep-0257/) -- Docstring conventions. For each class or method add a description with summary, input parameter, returned parameter,  type of parameter
    ```python
    def my_method(my_parameter):
        """
        Description of he method
        :param my_parameter: description of he parameter
        :type my_parameter: str
        """
    ```
    
2. Respect [PEP 8](https://www.python.org/dev/peps/pep-0008/) -- Style Guide for Python Code
We recommend the usage of an IDE like [Pycharm](https://www.jetbrains.com/pycharm/)

[Limitations]

3. The management of incoming variable from the signal order when they are __numbers or float are not efficient__.
    - Because of the differences between the STTs outputs: some are returning word some numbers (two != 2). 
    - Because of the i18n, we are not able to know if a variable should be  interpreted in english, french, spanish, etc ... ("two" != "deux" != "dos")
