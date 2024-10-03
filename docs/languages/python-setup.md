# :simple-python: Python Setup

## System Wide Python with `pyenv`

If you're on Linux or Mac, there is already Python installed and you can use `python` on the command line out of the box. 
However, it's usually out of date.

![Insert XKCD here](https://imgs.xkcd.com/comics/python_environment_2x.png)

But it's not advisable to upgrade your system python because all kinds of errors might pop up. 

??? warning "Viet's Story"

    At once point, I tried to upgrade my `python` system wide on an Ubuntu machine to the latest `python3` version.

    My bash terminal died/won't start up.

    As it turns out bash was relying on `python` being symlinked to `python2.7` and I've messed up.

    Just let smarter people figure it out for you.

**Solution**: Let [pyenv][1] manages system-wide python.

1. (Optional) read the README.md in its entirety
2. Install pyenv
    1. Don't forget to go through the "Setup shell environment for pyenv"
3. Install like the 2 latest version of Python3 (might need system packages). For example:
   ```{ ..console .copy }
   pyenv install 3.10 3.11
   ```

4. Usage:

    > To select a Pyenv-installed Python as the version to use, run one of the following commands:
    > 
    > 1. `pyenv shell <version>` -- select just for current shell session
    > 2. `pyenv local <version>` -- automatically select whenever you are in the current directory (or its subdirectories)
    > 3. `pyenv global <version>` -- select globally for your user account



[1]: https://github.com/pyenv/pyenv


