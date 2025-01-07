# Python

## pyenv 

Dev. environment setup.

- [Utilisation plusieurs versions de python avec Pyenv](https://blog.stephane-robert.info/post/python-pyenv-pipenv/)

## linter

[Python Code Quality: Tools & Best Practices](https://realpython.com/python-code-quality/)

### Error pip install

- Error message with `pip install`

```
$ pip install pwntools
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try 'pacman -S
    python-xyz', where xyz is the package you are trying to
    install.
    
    If you wish to install a non-Arch-packaged Python package,
    create a virtual environment using 'python -m venv path/to/venv'.
    Then use path/to/venv/bin/python and path/to/venv/bin/pip.
    
    If you wish to install a non-Arch packaged Python application,
    it may be easiest to use 'pipx install xyz', which will manage a
    virtual environment for you. Make sure you have python-pipx
    installed via pacman.

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.

```

- Soluce

```
The --break-system-packages flag in pip allows to override the externally-managed-environment error and install Python packages system-wide.

pip install package_name --break-system-packages

```