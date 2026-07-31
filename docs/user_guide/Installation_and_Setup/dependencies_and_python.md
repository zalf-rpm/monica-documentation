# Python Support and Dependencies

MONICA's simulation core is implemented in C++. Python is used by selected examples, producer/consumer workflows, and auxiliary service scripts. Python dependencies depend on the workflow being used.

---

## 1. Python Environment

A virtual environmment is recommended for Python-based examples:

```bash
python -m venv monica-env
```

Activate it with:

```
# Windows
monica-env\Scripts\activate

# Linux
source monica-env/bin/activate
```

Use a Python version compatible with the selected example and its external MONICA infrastructure dependencies. The MONICNA repository itself does not formally specify or test a Python range.

---

## 2. Python Dependencies

Insall only the dependencies required by the workflow.

**Producer/consumer examples**

The ZeroMQ examples use `pyzmq`:

```
python -m pip install pyzmq
```

**Examples using numerical array processing**

Some auxiliary scripts use NumPy:

```
python -m pip install numpy
```

**Cap'n Proto Python services**

The Cap'n Proto service examples import the `capnp` module, provided by the `pycapnp` package:

```
python -m pip install pycapnp
```

These examples may also require generated Python schema modules and supporting packages from the MONICA/MAS infrastucture repositories. Installing `pycapnp` alone may not be sufficient.

---

## 3. Native MONICA Dependencies

The `monica-run`, `monica-zmq-server`, and related executables are native C++ programs. They are built with CMake and require native build dependencies, including C++, ZeroMQ, Cap'n Proto, and the repositories or submodules specified by the build instructions.

Python packages are not required to run the native `monica-run` executable.

For containerized builds, see the repository's [Dockerfile](https://github.com/zalf-rpm/monica/blob/master/Dockerfile).

---

## 4. Running MONICA

After building MONICA, run a simulation with:

```
monica-run -o output.csv path/to/sim.json
```

The simulation configuration normally refers to a climate, site, and crop input files. The `MONICA_PARAMETERS` environment variable must point to the `monica-parameters` directory when required by the configuration.

---

## 5. Verifying the Installation

Verify the native MONICA executable with:

```
monica-run --version
```

The command should print the installed MONICA version.

For Python-based workflows, verify the installed Python packages with:

```
python -c "import zmq; print('pyzmq:', zmq.__version__)"
python -c "import numpy; print('numpy:', numpy.__version__)"
python -c "import capnp; print('pycapnp is available')"
```

Only run the checks for packages required by your selected workflow.

---

## 6. Updating Python Packages

If dependencies were installed manually, update them selectively:

```
python -m pip install --upgrade pyzmq numpy pycapnp
```

Avoid upgrading unrelated packages indiscriminately, because external MONICA and MAS infrastructure components may require specific compatible versions.

---

## 7. Docker and HPC

The repository contains a Docker build for the native MONICA services. When using Docker, Singularity, or another HPC container runtime, use the corresponding container definition and install Python packages inside the container only when running Python-based workflows.