# Running the notebook

1. Create a virtual environment: 

	```
	$ python3 -m venv venv
	```

2. Activate the environment:

	```
	$ source venv/bin/activate
	```

3. Update `pip`:

	```
	$ pip install --upgrade pip
	```

4. Install the modules:

	```
	$ pip install -r requirements.txt
	```

5. Clone PennyLane and checkout the `1a882984c8f5bc3ce13b74f8f395ce9262c0a975` commit (because of a bug with wires that would prevent the `pennylane-forest` plugin from running):

	```
	$ git clone https://github.com/XanaduAI/pennylane
	$ git checkout 1a882984c8f5bc3ce13b74f8f395ce9262c0a975
	```

6. Build PennyLane locally:

	```
	$ cd pennylane
	$ pip install -e .
	```

7. Launch Jupyter Notebook:

	```
	$ jupyter notebook
	```

8. Open and run `Notebook.ipynb`

# Running the pyQuil/Forest SDK examples

## Installing pennylane-forest

1. Clone the PennyLane Forest plugin:

	```
	$ git clone https://github.com/rigetti/pennylane-forest
	```

2. Build the plugin locally:

	```
	$ cd pennylane-forest
	$ pip install -e .
	```

## Running qvm and quilc

### Using Docker images

The easiest way is to run both `qvm` and `quilc` as Docker images.

1. Install Docker

2. Make sure no service is running on ports 5000 and 5555

3. To run both `qvm` and `quilc` interactively:

	1. Run the following on the console (may need `sudo`):

	```
	$ docker run --rm -it -p 5000:5000 rigetti/qvm -S
	```

	2. Open a new console and run (may need `sudo`):

	```
	$ docker run --rm -it -p 5555:5555 rigetti/quilc -R
	```

4. Otherwise, to run them in the background, add `-d` to both the previous commands

Note: The `qvm` process can crash due to memory issues; if that happens, try to run it with `--default-allocation foreign`. Performance can also improve by adding the `-c` (JIT compilation of Quil programs) parameter.

### Building QVM from the source

The QVM can also be built from the source.

1. Clone the QVM repository:

	```
	$ git clone https://github.com/rigetti/qvm
	```

2. Install the needed libraries, for instance on Ubuntu:

	```
	$ sudo apt-get update
	$ sudo apt-get install make pkg-config libffi-dev libblas-dev liblapack-dev sbcl
	```

3. Build QVM with `make`, optionally with a parameter for the memory:

	```
	$ cd qvm/
	$ make QVM_WORKSPACE=24576 qvm
	```

3. Run `qvm`:

	```
	$ ./qvm -S -c`
	```

