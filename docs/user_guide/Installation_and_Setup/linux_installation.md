# 2. Linux installation

## Compiling MONICA on Linux

Currently, MONICA does not provide a precompiled ZIP version for Linux.  
However, you can easily install and build it locally using the following steps (tested on Debian/Ubuntu systems).

---

### **1. Prerequisites**

Before building MONICA, make sure the following software is installed on the system:

- Python **3.6+**
- CMake
- Git
- Curl
- Unzip
- Tar

Install all dependencies on Debian/Ubuntu with:

```bash
sudo apt-get update
sudo apt-get install build-essential
sudo apt-get install python3-dev
sudo apt-get install curl unzip tar
sudo apt-get install git
sudo apt-get install cmake
sudo apt-get install libtool autoconf
```
These packages provide the basic tools and libraries required to build MONICA from source.

### **2. Create a working folder (e.g, `~/zalf-rpm`)**

Create a directory to store MONICA and its dependencies:

```bash
mkdir zalf-rpm
cd zalf-rpm
```

### **3. Clone MONICA and Dependencies**

Clone the MONICA source code along with its submodules and the parameter repository:

```bash
git clone --recurse-submodules https://github.com/zalf-rpm/monica.git
git clone https://github.com/zalf-rpm/monica-parameters.git
```

### **4. Setup Up vcpkg for Dependent Libraries**

MONICA uses vcpkg to manage external libraries.
Clone the vcpkg repository using the version tag specified in the `vcpkg_tag.txt` file found in the MONICA repository root.

```bash
git clone -b 2024.09.30 https://github.com/Microsoft/vcpkg.git
```

Build vcpkg:

```bash
cd vcpkg
./bootstrap-vcpkg.sh
```

### **5. Install Required Libraries via vcpkg**

Install the following dependencies: 

```bash
./vcpkg install zeromq:x64-linux
./vcpkg install capnproto:x64-linux
./vcpkg install libsodium:x64-linux
./vcpkg install tomlplusplus:x64-linux
```

These libraries enable communication, serialization, encryption, and configuration parsing within MONICA.

### **6. Build MONICA**

After installing all dependencies, build MONICA using its provided build script.

```bash
cd ~/zalf-rpm/monica
sh create_cmake_release.sh
cd _cmake_release
make
```
The script creates a CMake build directory named `_cmake_release` and compiles MONICA.
The resulting executable (`monica-run`) will be located inside this folder.

### **7. Run MONICA**

Once the build completes successfully, verify the installation:

```bash
./monica-run --help
```

If you see a list of available options and commands, MONICA has been built correctly.

### **8. Running a Test Simulation**
To perform a test run, user can use one of the included example simulation files, for instance:

```
./monica-run -o ../../output_csv/out.csv ../installer/Hohenfinow2/sim-min.json
```

This command runs a short test simulation and writes results to `output_csv/out.csv`.