# pyenv: set up pyenv with Poetry

This guide will be a short introduction how you can install pyenv and Poetry then use 
that to switch python environments locally.

## pyenv 

pyenv lets you easily switch between multiple versions of Python. 

You will need sudo permissions to download certain packages for the install to work:

```bash
sudo apt-get install -y build-essential 
libbz2-dev libncursesw5-dev libreadline-dev libffi-dev libssl-dev 
libgdbm-dev zlib1g-dev libsqlite3-dev liblzma-dev tk-dev uuid-dev
```
after that you can just run the below commands to get your pyenv working on your account without sudo:

```bash
curl https://pyenv.run | bash
```

Check out [pyenv github page](https://github.com/pyenv/pyenv) for what you need to export to get it working in your terminal for my set up in Ubuntu I ran:

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init - bash)"' >> ~/.bashrc
```

Then refresh your terminal and check it has installed
```bash
source ~/.bashrc
pyenv --version
```

To install Poetry it is recommend to have [pipx](https://github.com/pypa/pipx) installed and follow the poetry guidelines here(https://python-poetry.org/docs/)

Once you have a repo set up with your desired python version you can use pyenv with Poetry to change python versions locally:
```bash
pyenv install 3.9.8
pyenv local 3.9.8  # Activate Python 3.9 for the current project
poetry install
```

