# virtualenv
Keep your python environment clean

## why
* It will create a folder containing all the executable with packages
* It will sovle the python2 vs python3 problem in you local setup: 
    `virtualenv -p python3 env`
* When activated, any package you install will be place in the venv folder instead of global python installation
* And you can remove the env by simply delete the executable dir

## useful command
* To keep environment consistent, you can generate the requirement file by using `freeze`:

```bash
$ pip freeze > requirements.txt
$ pip install -r requirements.txt
```

## related tools
* autoenv: automagically activates the environment when you cd into the directory, to install:
```bash
$ git clone git://github.com/kennethreitz/autoenv.git ~/.autoenv
$ echo 'source ~/.autoenv/activate.sh' >> ~/.bashrc
```
