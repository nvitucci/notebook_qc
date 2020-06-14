# Optimization of a quantum circuit to prepare a Bell state

In this notebook I will show how to create and optimize a quantum circuit to prepare a [Bell state](https://en.wikipedia.org/wiki/Bell_state), the simplest two-qubit example of quantum entanglement. Step by step, we will see how to:

- create a quantum circuit with trainable parameters;
- define a quantum device;
- define the observables to use for measurement;
- train the circuit parameters towards a target state;
- visualize the parameters' evolution through the training steps.

I will use [PennyLane](https://pennylane.ai/), a Python library for quantum machine learning, throughout the notebook. I will also highlight my contributions to the library.

## Running the notebook

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

5. Clone PennyLane (since at this moment my contribution are only present on the master version):

	```
	$ git clone https://github.com/XanaduAI/pennylane
	```

	1. _(Only necessary if running the pyQuil/Forest SDK examples)_ Checkout the `1a882984c8f5bc3ce13b74f8f395ce9262c0a975` commit (because of [a bug I found](https://github.com/rigetti/pennylane-forest/issues/52) with wires that would prevent the `pennylane-forest` plugin from running):

		```
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

8. Open and run the `state_preparation_circuit_optimization.ipynb` notebook

## Running the pyQuil/Forest SDK examples

This is only necessary when running the notebook with Rigetti QVM (the `forest.qvm` device) and QPU (the `forest.qpu` device). The two devices are included but commented out in the notebook.

### Installing pennylane-forest

1. Clone the PennyLane Forest plugin:

	```
	$ git clone https://github.com/rigetti/pennylane-forest
	```

2. Build the plugin locally (since the new Aspen-8 device is only present on the master version):

	```
	$ cd pennylane-forest
	$ pip install -e .
	```

### Running `qvm` and `quilc`

The plugin requires both the quantum virtual machine (`qvm`) and the Quil compiler (`quilc`) to be running. The easiest way to run both is by using Docker images; on the other hand, if the configuration of the QVM needs to be customized (for instance to assign more memory to the process), it is necessary to build it from the source.

#### Using Docker images

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

#### Building QVM from the source

1. Clone the QVM repository:

	```
	$ git clone https://github.com/rigetti/qvm
	```

2. Install the needed libraries, for instance on Ubuntu:

	```
	$ sudo apt-get update
	$ sudo apt-get install make pkg-config libffi-dev libblas-dev liblapack-dev sbcl
	```

3. Build with `make`, optionally with a parameter for the memory:

	```
	$ cd qvm/
	$ make QVM_WORKSPACE=24576 qvm
	```

3. Run `qvm` (the `-c` parameter, which enables Quil programs JIT compilation, is optional but seems to improve the response speed):

	```
	$ ./qvm -S -c`
	```

## Acknowledgments

Big thanks go to:

- [Tom Bromley](https://github.com/trbromley) (Xanadu), for all the interesting discussions about PennyLane extensions, for guiding me through conventions and existing work, and for reviewing my code and notebook;
- [Josh Izaac](https://github.com/josh146) (Xanadu), for putting me in touch with the PennyLane team, giving many useful suggestions, and for reviewing my code;
- [Tom Lubowe](https://github.com/tlubowe) (Rigetti), for being my mentor, supporting the project, and raising issues on the QVM interaction;
- [Michał Stęchły](https://github.com/mstechly) (QOSF), for organizing the mentorship program and creating the inspiring QOSF community.
