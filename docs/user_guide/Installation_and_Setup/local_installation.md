# Windows installation

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

ii.	 Create a new environment variable named MONICA_PARAMETERS and set its value to: C:\monica\monica-parameters

iii. Open a new Command Prompt and verify the installation: monica-run.exe - -help (In some versions, the executable is called monica.exe, whichever appears in the bin folder)

**Note: Some releases include only monica-run.exe (and not monica.exe). This is normal as both serve the same purpose for running simulations.**


## 3.Updating an Existing Installation

- If the user already has an older MONICA installation (for example under
%USERPROFILE%\AppData\MONICA), user can simply replace the files in the existing bin folder with those from the new ZIP.
- However, it’s generally recommended to use the ZIP-based workflow exclusively. This avoids version conflicts and allows each project to reference its own MONICA version explicitly.

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
