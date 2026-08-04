# Compiling MONICA on Windows

## Requirements

- Windows 10 or newer
- Git
- CMake 3.22 or newer
- VisualStudio 2022 or Visual Studio 2022 Build Tools
    - Desktop development with C++
    - MSVC compiler
    - Windows SDK
    - English language pack
- Python, if you want to run the Python-based examples
- NSIS, if you want to create an installer

The full Visual Studio IDE is not required. Visual Studio Build Tools is sufficient for command-line builds.

Verify the tools from PowerShell:
```powershell
git --version
cmake --version
```

When using Visual Studio Build Tools, run the commands from a **Developer PowerShell for VS 2022**, or ensure that `msbuild.exe` is available on `PATH`.

---

## Required environment variables

The following executable directories must be available on `PATH`:

- Git, for example: `C:\Program Files\Git\cmd`
- CMake, for example: `C:\Program Files\CMake\bin`
- MSBuild, if it is not being run from a Visual Studio Developer PowerShell

---

## 1. Clone the repositories

Create a working directory and clone MONICA and its related repositories:

```powershell
mkdir C:\zalf-rpm
cd C:\zalf-rpm
git clone --recurse-submodules https://github.com/zalf-rpm/monica.git
git clone https://github.com/zalf-rpm/monica-parameters.git
git clone https://github.com/zalf-rpm/mas-infrastructure.git
```

The `mas-infrastructure` repository is required when creating the Windows installer.

The resulting directory structure should be:

```
C:\zalf-rpm
├── monica
├── monica-parameters
├── mas-infrastructure
└── vcpkg
```

---

## 2. Install vcpkg dependencies

The required vcpkg version is stored in `monica/vcpkg_tag.txt`. Clone that exact version:

```powershell
cd C:\zalf-rpm
git clone -b 2025.10.17 https://github.com/Microsoft/vcpkg.git
```

Bootstrap vcpkg:

```powershell
cd C:\zalf-rpm\vcpkg
.\bootstrap-vcpkg.bat
```

Install the required libraries:

```powershell
 .\vcpkg install zeromq:x64-windows-static
 .\vcpkg install capnproto:x64-windows-static
 .\vcpkg install libsodium:x64-windows-static
 .\vcpkg install tomlplusplus:x64-windows-static
```

---

## 3. Generate the Visual Studio solution

Change to the MONICA repository:

```powershell
cd C:\zalf-rpm\monica
```

Generate the 64-bit Visual Studio 2022 solution:

```powershell
.\create_solution_x64_VS17_2022.cmd
```

This creates the following build directory:

```powershell
C:\zalf-rpm\monica\_cmake_win64
```

The generated solution is:

```powershell
C:\zalf-rpm\monica\_cmake_win64\monica.sln
```

---

## 4. Build MONICA

**Using Visual Studio**

Open the solution:

```
start .\_cmake_win64\monica.sln
```

In Visual Studio:

1. Select the `x64` platform.
2. Select `Release` or `Debug`.
3. Build the solution.

For creating an installer, select `Release`.

**Using MSBuild**

Build a Release version from a Developer PowerShell:

```
msbuild _cmake_win64/monica.sln /p:Configuration=Release /p:Platform="x64"
```

Alternatively, use CMake directly:

```
cmake --build _cmake_win64 --config Release
```

The Release executables are placed in:

```
C:\zalf-rpm\monica\_cmake_win64\Release
```

---

## 5. Run the local example

From the MONICA repository root:

```
cd C:\zalf-rpm\monica .\_cmake_win64\Release\monica-run.exe -o .\installer\Hohenfinow2\sim-min.json
```

If the example requires parameter files, set the parameter directory:

```
$env:MONICA_PARAMETERS = "C:\zalf-rpm\monica-parameters"
```

---

## 6. Creating a Windows installer

**Prerequisites**

Before creating an installer:

- Build MONICA using the `Release` configuration.
- Clone `monica-parameters`.
- Clone `mas-infrastructure`.
- Install NSIS.
- Ensure the following files exist:
```
monica\_cmake_win64\Release\monica-run.exe
monica-parameters\
mas-infrastructure\src\python\services\soil\soil_io3.py
```

**Build the installer**

The repository provides a helper script:

```
\installer\run-installer-with-parameter.bat
```

Run it from the installer directory:

```
cd C:\zalf-rpm\monica\installer
.\run-installer-with-parameter.bat 3.4.0 213 x64
```

The arguments are:

```
run-installer-with-parameter.bat <version> <build-number> <architecture>
```

The generated installer is written to the `installer` directory.