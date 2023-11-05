## 20231103

https://pytorch.org/tutorials/beginner/blitz/tensor_tutorial.html

* Create an environment and install packages / requirement. According to the link, it requires torch, torchvision and matplotlib. 
    * Create a backup script first (zip to a folder)
* `torch.cuda.is_available()` is `False`
* Trying to use the GPU
    * https://medium.com/mlearning-ai/mac-m1-m2-gpu-support-in-pytorch-a-step-forward-but-slower-than-conventional-nvidia-gpu-40be9293b898
    ```python
    import torch
    import math

    print(torch.backends.mps.is_available()) #the MacOS is higher than 12.3+
    print(torch.backends.mps.is_built()) #MPS is activated
    ```
    * https://developer.apple.com/metal/pytorch/
        * Without installing `pytorch-nightly`, mps already seems available: `torch.backends.mps.is_available()` is `True`, and 
        ```python
        import torch
        if torch.backends.mps.is_available():
            mps_device = torch.device("mps")
            x = torch.ones(1, device=mps_device)
            print (x)
        else:
            print ("MPS device not found.")
        ```
        gives `tensor([1.], device='mps:0')`



Getting requirements to build wheel ... error
error: subprocess-exited-with-error
This error originates from a subprocess, and is likely not a problem with pip

```sh
Getting requirements to build wheel ... error
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [45 lines of output]


      WARNING, No "Setup" File Exists, Running "buildconfig/config.py"
      Using Darwin configuration...

      /bin/sh: sdl2-config: command not found
      /bin/sh: sdl2-config: command not found
      /bin/sh: sdl2-config: command not found

      ---
      For help with compilation see:
          https://www.pygame.org/wiki/MacCompile
      To contribute to pygame development see:
          https://www.pygame.org/contribute.html
      ---

      Traceback (most recent call last):
        File "/Users/lichaowang/work/software/python_venvs/modeling/lib/python3.11/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/lichaowang/work/software/python_venvs/modeling/lib/python3.11/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/lichaowang/work/software/python_venvs/modeling/lib/python3.11/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 118, in get_requires_for_build_wheel
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/dt/f3mjt8_92wjb2kv63r3v1r8r0000gn/T/pip-build-env-rxzh35rb/overlay/lib/python3.11/site-packages/setuptools/build_meta.py", line 355, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/dt/f3mjt8_92wjb2kv63r3v1r8r0000gn/T/pip-build-env-rxzh35rb/overlay/lib/python3.11/site-packages/setuptools/build_meta.py", line 325, in _get_build_requires
          self.run_setup()
        File "/private/var/folders/dt/f3mjt8_92wjb2kv63r3v1r8r0000gn/T/pip-build-env-rxzh35rb/overlay/lib/python3.11/site-packages/setuptools/build_meta.py", line 507, in run_setup
          super(_BuildMetaLegacyBackend, self).run_setup(setup_script=setup_script)
        File "/private/var/folders/dt/f3mjt8_92wjb2kv63r3v1r8r0000gn/T/pip-build-env-rxzh35rb/overlay/lib/python3.11/site-packages/setuptools/build_meta.py", line 341, in run_setup
          exec(code, locals())
        File "<string>", line 359, in <module>
        File "/private/var/folders/dt/f3mjt8_92wjb2kv63r3v1r8r0000gn/T/pip-install-4m72mvfw/pygame_88a9ce4282ba4f748579a1299a63d800/buildconfig/config.py", line 225, in main
          deps = CFG.main(**kwds)
                 ^^^^^^^^^^^^^^^^
        File "/private/var/folders/dt/f3mjt8_92wjb2kv63r3v1r8r0000gn/T/pip-install-4m72mvfw/pygame_88a9ce4282ba4f748579a1299a63d800/buildconfig/config_darwin.py", line 132, in main
          [DependencyProg('SDL', 'SDL_CONFIG', 'sdl2-config', '2.0', ['sdl'])],
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/dt/f3mjt8_92wjb2kv63r3v1r8r0000gn/T/pip-install-4m72mvfw/pygame_88a9ce4282ba4f748579a1299a63d800/buildconfig/config_unix.py", line 39, in __init__
          self.ver = config[0].strip()
                     ~~~~~~^^^
      IndexError: list index out of range
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error

× Getting requirements to build wheel did not run successfully.
│ exit code: 1
╰─> See above for output.

note: This error originates from a subprocess, and is likely not a problem with pip.
```

In the end, made 2 changes for setting up the complete environment: 
1. switched back to python@3.10 for the above problem
2. changed ray's version to 2.5.1

`num_workers=2` in DataLoader is causing problems. (`num_workers=0` is fine)
```sh
Traceback (most recent call last):
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/multiprocessing/spawn.py", line 125, in _main
    prepare(preparation_data)
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/multiprocessing/spawn.py", line 236, in prepare
    _fixup_main_from_path(data['init_main_from_path'])
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/multiprocessing/spawn.py", line 287, in _fixup_main_from_path
    main_content = runpy.run_path(main_path,
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/runpy.py", line 289, in run_path
    return _run_module_code(code, init_globals, run_name,
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/runpy.py", line 96, in _run_module_code
    _run_code(code, mod_globals, init_globals,
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/runpy.py", line 86, in _run_code
    exec(code, run_globals)
  File "/Users/lichaowang/work/projects/learning/coding/python/models/pytorch-tutorials/beginner_source/blitz/cifar10_tutorial.py", line 111, in <module>
    dataiter = iter(trainloader)
  File "/Users/lichaowang/work/software/python_venvs/modeling/lib/python3.10/site-packages/torch/utils/data/dataloader.py", line 438, in __iter__
    return self._get_iterator()
  File "/Users/lichaowang/work/software/python_venvs/modeling/lib/python3.10/site-packages/torch/utils/data/dataloader.py", line 386, in _get_iterator
    return _MultiProcessingDataLoaderIter(self)
  File "/Users/lichaowang/work/software/python_venvs/modeling/lib/python3.10/site-packages/torch/utils/data/dataloader.py", line 1039, in __init__
    w.start()
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/multiprocessing/process.py", line 121, in start
    self._popen = self._Popen(self)
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/multiprocessing/context.py", line 224, in _Popen
    return _default_context.get_context().Process._Popen(process_obj)
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/multiprocessing/context.py", line 288, in _Popen
    return Popen(process_obj)
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/multiprocessing/popen_spawn_posix.py", line 32, in __init__
    super().__init__(process_obj)
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/multiprocessing/popen_fork.py", line 19, in __init__
    self._launch(process_obj)
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/multiprocessing/popen_spawn_posix.py", line 42, in _launch
    prep_data = spawn.get_preparation_data(process_obj._name)
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/multiprocessing/spawn.py", line 154, in get_preparation_data
    _check_not_importing_main()
  File "/opt/homebrew/Cellar/python@3.10/3.10.13_1/Frameworks/Python.framework/Versions/3.10/lib/python3.10/multiprocessing/spawn.py", line 134, in _check_not_importing_main
    raise RuntimeError('''
RuntimeError: 
        An attempt has been made to start a new process before the
        current process has finished its bootstrapping phase.
        This probably means that you are not using fork to start your
        child processes and you have forgotten to use the proper idiom
        in the main module:
            if __name__ == '__main__':
                freeze_support()
                ...
        The "freeze_support()" line can be omitted if the program
        is not going to be frozen to produce an executable.
python-BaseException
```

OK. Actually wrapping everything into a main program as said in the error msg helped: 
```python
if __name__ == '__main__':
    main()
```