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

## 20231105

CPU training fine on cifar-10. Check GPU training (once ok, go to difusion modeling)
Note that with CPU, each epoch takes a bit more than 27 seconds (27.66s and 27.23s for both epochs). Training data is 50k images split into batch_size 4 (so 12.5k batches). With a single mps GPU, it took much longer. Every epoch took about 1min (62s and 59s for 2 epochs). 
These used `n_channels_1 = 6` and `n_channels_2 = 16`. 

After changing `n_channels_1 = 300` and `n_channels_2 = 300`, an epoch takes a little more than 3mins on CPU (186s and 187s for 2 epochs), while about 87 seconds on GPU (89s and 87s for 2 epochs). Considering the data loading overhead is at least (60s - 27s = 33s), the training speeds of GPU vs CPU is `(88-33) / 186 ~ 30%`. Now using 30% to the first experiment, then the data loading overhead becomes 60s - 9s = 51s, and putting this updated overhead to the 2nd experiment, the speeds comparison becomes `(88-51)/186 ~ 20%`. So the GPU is at least 5x faster (even this is an underestimate). Also this is using a single GPU. 

//hl One thing to note is that even with the single GPU for training for 2 epochs about less than 3 mins in total, the laptop becomes very hot, and there were noises of things popping/cracking. So probably shouldn't use this laptop on GPU training for too long or too many GPUs. 

One minor problem I couldn't solve was how to "mirror push" the pytorch tutorial repo to my private repo. The main reason is that the repo is too big (>5G), and there's various limitations on github. I tried using various ssh configs, as well as https, but couldn't get a success. Note that the direct fork from the public repo has to stay public (including my branches pushed to that fork). 

//fix another thing is the author name / email, which is 
Lichao Wang <lichaowang@Lichaos-MacBook-Pro.local>
That's too much private info.

`git config --global user.name "flyinggip"`
`git config --global user.email "flyinggip@blank.org"`

