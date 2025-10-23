# 1. Windows installation

## Installing and Accessing the ZIP Version of MONICA under Windows

Starting with **version 3.6.x**, MONICA is distributed as a **ZIP archive** instead of a traditional installer.  
This makes setup simpler and more flexible where users can keep multiple MONICA versions on their computer and easily run a specific one for each project.

---

## 1. Extracting MONICA

- **Download** the latest Windows ZIP package from the [MONICA Releases Page](https://github.com/zalf-rpm/monica/releases), for example: **`monica-windows-x86_64.zip`**

- **Extract** the contents to a convenient location, such as: `C:\monica\`


- After extraction, folder structure should look like this:

```
C:\monica
├── bin
├── monica-parameters
├── projects
└── documentation\
```
There is no installation process. Simply unzipping the archive is enough to make MONICA ready for use. User may also maintain several versions in parallel (e.g., C:\monica_3.6.12\, C:\monica_3.7.0\).

---

## 2. Running MONICA

Because the ZIP version isn’t registered automatically in Windows, user can either:

- Run the included example project, or

- Add MONICA to the user system PATH to use it from anywhere.

### **Option 1: Run the Example Project (Recommended)**

MONICA comes with a ready-to-run example project.

i.	Open Command Prompt (Win + R → type cmd → press Enter).

ii.	Type the following commands:

```
C:\monica\projects\Hohenfinow2-single-run
```

iii. Run the simulation by typing:

```
run_monica.cmd
```

The script automatically:

- Points to the MONICA executable in the bin folder
- Sets required environment variables (e.g., MONICA_PARAMETERS)
- Runs a predefined example simulation


When the run is complete, a file named `sim-out.csv` will appear in the same folder.
Open it in `Excel` or any text editor to view the results.


### **Option 2: Run MONICA Manually (Advanced)**

If user prefer to run MONICA from anywhere on the system

i.	 Add the path to bin folder (e.g. C:\monica\bin) to the PATH environment variable

ii.	 Create a new environment variable named MONICA_PARAMETERS and set its value to: `C:\monica\monica-parameters`

iii. Open a new Command Prompt and verify the installation: `monica-run.exe - -help` 
(In some versions, the executable is called monica.exe, whichever appears in the bin folder)

**Note: Some releases include only monica-run.exe (and not monica.exe). This is normal as both serve the same purpose for running simulations.**

---

## 3.Updating an Existing Installation

- If the user already has an older MONICA installation **(for example under%USERPROFILE%\AppData\MONICA)**, user can simply replace the files in the existing bin folder with those from the new ZIP.

- However, it’s generally recommended to use the ZIP-based workflow exclusively. This avoids version conflicts and allows each project to reference its own MONICA version explicitly.

---

## 4. Verifying the Installation
- To confirm that MONICA runs correctly, enter the script in command prompt : 

```
C:\monica\projects\Hohenfinow2-single-run
```

Run the simulation by typing:

```
run_monica.cmd
```

If the setup is correct, the model will complete a short test simulation and generate an output file named `sim-out.csv` in the same directory. This confirms that MONICA is successfully installed and functional.


---

# 2. Linux installation

## Local Installation of MONICA on Linux

Currently, MONICA does not provide a precompiled ZIP version for Linux.  
However, user can easily install and build it locally using the following steps (tested on Debian/Ubuntu systems).

---

### **1. Prerequisites**

Before building MONICA, make sure the following software is installed on the system:

- Python **3.6+**
- CMake
- Git
- Curl
- Unzip
- Tar

install all dependencies on Debian/Ubuntu with:

```
sudo apt-get update
sudo apt-get install build-essential
sudo apt-get install python3-dev
sudo apt-get install curl unzip tar
sudo apt-get install git
sudo apt-get install cmake
sudo apt-get install libtool autoconf
```
These packages provide the basic tools and libraries required to build MONICA from source.

### **2. Create a working folder (e.g ~/zalf-rpm)**

Create a directory to store MONICA and its dependencies:

```
mkdir zalf-rpm
cd zalf-rpm
```

### **3. Clone MONICA and Dependencies**

Clone the MONICA source code along with its submodules and required parameters:

```
git clone --recurse-submodules https://github.com/zalf-rpm/monica.git
git clone https://github.com/zalf-rpm/monica-parameters.git
```
### **4. Setup vcpkg for Dependent Libraries**

MONICA uses vcpkg to manage external libraries.
Clone the vcpkg repository using the version tag specified in the **vcpkg_tag.txt** file found in the MONICA repository root. This initializes vcpkg and prepares it to build required libraries

```
git clone -b 2024.09.30 https://github.com/Microsoft/vcpkg.git
```
build vcpkg by typing:

```
cd vcpkg
./bootstrap-vcpkg.sh
```
### **5. Install Required Libraries via vcpkg**

Use vcpkg to install the following dependencies: 

```
./vcpkg install zeromq:x64-linux
./vcpkg install capnproto:x64-linux
./vcpkg install libsodium:x64-linux
./vcpkg install tomlplusplus:x64-linux
```
These libraries enable communication, serialization, encryption, and configuration parsing within MONICA.

### **6. Build MONICA**

After installing all dependencies, build MONICA using its provided build script.

```
cd ~/zalf-rpm/monica
sh create_cmake_release.sh
cd _cmake_release
make
```
The script creates a CMake build directory named `_cmake_release` and compiles MONICA.
The resulting executable `(monica-run)` will be located inside this folder.

### **7. Run MONICA**

Once the build completes successfully, verify the installation:

```
./monica-run --help
```

If the user see a list of available options and commands, MONICA has been built correctly.

### **8. Running a Test Simulation**
To perform a test run, user can use one of the included example simulation files, for instance:

```
./monica-run -o ../../output_csv/out.csv ../installer/Hohenfinow2/sim-min.json
```

This command runs a short test simulation and writes results to **output_csv/out.csv**