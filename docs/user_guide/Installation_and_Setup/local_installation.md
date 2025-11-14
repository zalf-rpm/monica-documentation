# 1. Windows installation

## Installing and Accessing the ZIP Version of MONICA on Windows

Starting with **version 3.6.12**, MONICA is distributed as a **ZIP archive** instead of a traditional installer.  
This makes the setup simpler and allows users to keep multiple MONICA versions on their computer, using a specific one for each project.

---

## 1. Extracting MONICA

- **Download** the latest Windows ZIP package from the [MONICA Releases Page](https://github.com/zalf-rpm/monica/releases), for example: **`monica_win64_3.6.32.toth_ser_TUA.zip`**

- **Extract** the contents to a convenient location, such as: `C:\monica\`

- After extraction, the folder structure should look like this:
```
C:\monica
├── bin
├── documentation
├── monica-parameters
└── projects
```

There is no installation process. Simply unzipping the archive is enough to make MONICA ready to use. Users may also keep multiple versions in parallel (e.g., `C:\monica_3.6.12\`, `C:\monica_3.6.27\`).

---

## 2. Running MONICA

Because the ZIP version is not automatically registered in Windows, users can either:

- Run the included example project, or

- Add MONICA to the user system PATH to run it from anywhere.

### **Option 1: Run the Example Project (Recommended)**

MONICA comes with a ready-to-run example project.

1. Open Command Prompt (Win + R → type `cmd` → press Enter).

2. Navigate to the example folder:
```
cd C:\monica\projects\Hohenfinow2-single-run
```

3. Run the simulation by typing:
```
run_monica.cmd
```

The script automatically:

- Points to the MONICA executable in the `bin` folder
- Sets required environment variables (e.g., `MONICA_PARAMETERS`)
- Runs a predefined example simulation

When the run is complete, a file named `sim-out.csv` will appear in the same folder.
Open it in `Excel` or any text editor to view the results.


### **Option 2: Run MONICA Manually (Advanced)**

If users prefer to run MONICA from anywhere on the system:

1. Add the `bin` folder to the PATH, e.g.:
```
C:\monica\bin
```

2. Set the `MONICA_PARAMETERS` environment variable to point to: 
```
C:\monica\monica-parameters
```

3. Open a new Command Prompt and verify the installation: 
```
monica-run.exe - -help
```

---

## 3. Updating an Existing Installation

- If the user already has an older MONICA installation, usually under `%USERPROFILE%\AppData\MONICA`, the user can simply replace the files in the existing `bin` folder with those from the new ZIP.

- However, it is generally recommended to use the ZIP-based workflow exclusively. This avoids version conflicts and allows each project to reference its own MONICA version explicitly.

- The ZIP-based approach will now be the default method for running MONICA.

---

## 4. Verifying the Installation
- To confirm that MONICA runs correctly: 

1. Navigate to the example folder:
```
C:\monica\projects\Hohenfinow2-single-run
```

2. Run the simulation by typing:
```
run_monica.cmd
```

If the setup is correct, the model will complete a short test simulation and generate an output file named `sim-out.csv` in the same directory. This confirms that MONICA is successfully installed and functional.

---

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

To perform a test run, use one of the included example simulation files, for instance:

```
./monica-run -o ../../output_csv/out.csv ../installer/Hohenfinow2/sim-min.json
```

This command runs a short test simulation and writes results to `output_csv/out.csv`.