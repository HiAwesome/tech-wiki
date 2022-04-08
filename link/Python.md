# Python

#### [MacOS: ModuleNotFoundError: No module named '_ctypes'](https://stackoverflow.com/a/69187767)

```shell

# Install x86 brew
arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
alias ibrew=/usr/local/bin/brew

# Install Python 3.7
arch -x86_64 ibrew install python@3.7

# Add `python` executable (symlink to `python3`)
ln -s python3 "$(ibrew --prefix python@3.7)"/bin/python

# Symlink x86 Python 3.7 into pyenv
ln -s "$(ibrew --prefix python@3.7)" .pyenv/versions/3.7.10

# Check
pyenv local 3.7.10
python -V
# Python 3.7.10
python -c 'import _ctypes'. # works!

```


## [pycodestyle](https://github.com/PyCQA/pycodestyle)

可以使用 pycodestyle 检查代码风格问题，用 [black](https://github.com/psf/black) 进行全部修复。

## pyenv

* pyenv install --list
* pyenv install 2.7.18
* pyenv install 3.9.10
* pyenv install 3.10.2
