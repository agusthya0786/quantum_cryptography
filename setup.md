# environment

* Rasbperry Pi running Raspbian or similar
* Python  ≥3.9 (required for type hinting)
  * highly recommended: a virtual environment (like [venv](https://docs.python.org/3/library/venv.html))
* [JupyterLab notebook](https://jupyter.org/) for the Web UI

# Raspbian setup

```sh
# check current Python version:
python3 --version # we need at least 3.9.

# build a newer Python version from source.
sudo apt update
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libssl-dev libffi-dev libsqlite3-dev

wget https://www.python.org/ftp/python/3.9.5/Python-3.9.5.tgz # or newer.
tar -xf Python-* && cd Python-*/

# these steps might take a while.
./configure --enable-optimizations && make -j 5 # ~ number of cores +1.
sudo make altinstall # altinstall leaves the original install alone.

# check if it worked:
python3.9 --version
python3.9 -c 'import ssl;print(ssl.OPENSSL_VERSION)'
```

# cloning and initial setup

```sh
# clone the repo:
git clone 'git@github.com:bmedicke/quantum_cryptography.git' # or via https.

# switch to the repo folder:
cd quantum_cryptography

# create a virtual Python environment with the new Python:
python3.9 -m venv env

# activate the virtual environment.
source env/bin/activate
  # while working in an activated virtual environment you can
  # simply use `python` and `pip` and  skip the postfixes.

# install requirements.
pip install wheel
pip install -r requirements.txt

# enable the I2C interface for the relay shield:
sudo raspi-config
  # Interfacing options → I2C → yes.
```

# starting the notebook

```sh
# activate the virtual environment if its not still active.
source env/bin/activate # you can deactivate the venv with: deactivate
jupyter-lab --ip=0.0.0.0 --no-browser notebooks/ # start JupyterLab.
  # the first flag binds the programm to all network interfaces so we
  # can connect to the raspberry pi via its public IP address.
  # the second flag prevents a browser from popping up.

# now visit the displayed URL with the public IP address
# from any browser running on a device in the same network.
```

All required libraries are installed into the virtual environment.<br>
**You have to activate the virtual environment before starting JupyterLab.**

---

To update the requirements file (after addding new libraries to the project)
run `pip freeze > requirements.txt`.

# libraries

* [relay_lib_seeed](https://github.com/johnwargo/seeed-studio-relay-board)
  * library to control the seeed relay hat
  * updated version: https://github.com/johnwargo/seeed-studio-relay-v2
    * supports stacked hats 🎩
* smbus
* jupyterlab_code_formatter and black

## configuring the code formatter

Set the default code formatter to black:

*Settings* → *Advanced Code Formatter* → *Jupyterlab Code Formatter* → *User Preferences*

```json
{
    "preferences": {
        "default_formatter": {
            "python": "black"
        }
    },
    "black": {
         "line_length": 79,
         "string_normalization": false
    }
}
```