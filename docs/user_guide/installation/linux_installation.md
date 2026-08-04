# Compiling MONICA on Linux

MONICA can be built from source on Linux. The instructions below are intended for Debian- or Ubuntu-based systems.

---

### **1. Prerequisites**

Install the required build tools:

```bash
sudo apt-get update
sudo apt-get install build-essential
sudo apt-get install curl
sudo apt-get install unzip
sudo apt-get install tar
sudo apt-get install git
sudo apt-get install cmake
sudo apt-get install libtool
sudo apt-get install autoconf
```

Python 3 is optional for the Python-based examples and tools:

```bash
sudo apt-get install python3
```

MONICA requires CMake 3.22 or newer and a C++17-compatible compiler.

### **2. Create a working directory**

```bash
mkdir ~/zalf-rpm
cd ~/zalf-rpm
```

### **3. Clone MONICA and its parameter repository**

Clone MONICA together with its submodules:

```bash
git clone --recurse-submodules https://github.com/zalf-rpm/monica.git
git clone https://github.com/zalf-rpm/monica-parameters.git
```

The resulting directory structure should be:

```
~/zalf-rpm/
├── monica/
└── monica-parameters/
```

### **4. Install vcpkg**

MONICA uses vcpkg to manage external C++ libraries. The required vcpkg version is stored in `monica/vcpkg_tag.txt`.

```bash
cd ~/zalf-rpm
git clone -b "$(cat monica/vcpkg_tag.txt)" https://github.com/Microsoft/vcpkg.git
cd vcpkg
./bootstrap-vcpkg.sh
```

### **5. Install MONICA dependencies**

For a 64-bit Linux system, install the required libraries using the `x64-linux` triplet: 

```bash
./vcpkg install zeromq:x64-linux
./vcpkg install capnproto:x64-linux
./vcpkg install libsodium:x64-linux
./vcpkg install tomlplusplus:x64-linux
```

### **6. Configure and build MONICA**

The provided script creates the CMake build directory and configures the project:

```bash
cd ~/zalf-rpm/monica
sh create_cmake_release.sh
```

Compile the project:

```bash
cmake _cmake_release
make
```

The main executable will be created at:

```bash
~/zalf-rpm/monica/_cmake_release/monica-run
```

### **7. Verify the build**

```bash
cd ~/zalf-rpm/monica/_cmake_release
./monica-run --help
```

A list of available command-line options indicates that the build completed successfully.

### **8. Configure the MONICA parameter path**

MONICA's example simulations use files from the `monica-parameters` repository. Set the `MONICA_PARAMETERS` environment variable:

```bash
export MONICA_PARAMETERS="$HOME/zalf-rpm/monica-parameters"
```

To set this variable automatically for future shell sessions, add the command to `~/.bashrc` or the corresponding shell configuration file.

### **9. Run a test simulation**

Create a directory for simulation output:

```bash
mkdir -p ~/zalf-rpm/output_csv
```

Run the included Hohenfinow2 example:

```bash
cd ~/zalf-rpm/monica
./_cmake_release/monica-run -o ~/zalf-rpm/output_csv/out.csv installer/Hohenfinow2/sim-min.json
```

If the simulation completes successfully, the result will be written to: `~/zalf-rpm/output_csv/out.csv`.