If you look for the older version you will find it here [How to compile MONICA?(old)](wiki/How-to-compile-MONICA-old) 

# Windows

## Required Software

* VisualStudio 2017 (community edition)
* Boost (Version 1.66.0)
* Python 2.7 (use Python27-x86)
* cmake 3.6 or newer
* git


## Environment Variables
make sure environment variables are set, usually this is done during installation of the programm
'Path' should contain the executable paths to:
* Python27-x86 (e.g. C:\Python27-x86\;C:\Python27-x86\Scripts)
* git (2.13 or higher) (e.g. C:\Program Files\Git\cmd)
* cmake (e.g. C:\Program Files\CMake\bin)

## Steps
* checkout monica-master with git
    >` git clone --recurse-submodules https://github.com/zalf-rpm/monica-master.git`

    **Note: ** --recurse-submodules parameter will checkout all required sub modules, without this parameter these folders will be empty
    Previous versions of git used different parameters. If you are using < 2.13 parameters are diffent. 

* download boost from https://www.boost.org/ (older versions are available on sourceforge, but should be linked on this website)
* unzip boost, you will probably get something like this folder structure

   |_ boost_1_66_0

        |_ boost

        |_ doc

        |_ lib

        |_ ...

* you can either copy the boost_1_66_0 folder into the  monica-master folder or create a symlink
    `> mklink /D boost <your boost path>` (e.g. mklink /D boost C:\boost_1_66_0)

Your folder structure should look like that:
`monica-master`
		`|_ monica`

		`|_ sys-lib`

		`|_ util`

		`|_ monica-parameters`

		`|_ boost`

			`|_ boost`

			`|_ doc`

			`|_ lib`

			`|_ ...`


* create a visual studio solution by using cmake
    change to monica-master/monica folder
    * execute update_solution.cmd to build a 32 bit version
    * update_solution_x64.cmd to build a 64 bit version
* change to the newly created _cmake_win32 folder or _cmake_win64
* open monica.sln to compile

# Linux / Debian 9 ('stretch')

gcc 
python 2.7 
cmake 
boost (currently in version 1.62 for debian)
zmq 
git

 ### note:
   many packages are pre-installed

sudo apt-get install python
sudo apt-get install libboost-all-dev
sudo apt-get install libczmq-dev
sudo apt-get install gcc build-essential 

* checkout monica-master
> git clone --recurse-submodules https://github.com/zalf-rpm/monica-master.git
* change to monica-master
> cd monica-master
* create a cmake directory and cmake build files
> mkdir -p _cmake_linux
> cd _cmake_linux
> cmake ../monica
* build
> make


