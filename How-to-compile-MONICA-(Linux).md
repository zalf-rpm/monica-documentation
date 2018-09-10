# Linux / Debian 9 ('stretch')

## Prequisites
* gcc 
* python 2.7 
* cmake 
* boost (currently in version 1.62 for debian) 
* git
* curl 
* unzip 
* tar

  sudo apt-get update
  sudo apt-get install build-essential
  sudo apt-get install python-dev
  sudo apt-get install libboost-all-dev
  sudo apt-get install curl unzip tar
  sudo apt-get install git
  sudo apt-get install cmake

* create a working folder (e.g ~/zalf-rpm)

  mkdir zalf-rpm
  cd zalf-rpm 

* checkout monica and dependencies 

  git clone https://github.com/zalf-rpm/monica.git
  git clone https://github.com/zalf-rpm/monica-parameters.git
  git clone https://github.com/zalf-rpm/sys-libs.git
  git clone https://github.com/zalf-rpm/util.git

  git clone https://github.com/Microsoft/vcpkg.git

* build vcpkg
  
  cd vcpkg
  ./bootstrap-vcpkg.sh
 
* build zmq lib
  
  ./vcpkg install zeromq:x64-linux

* create monica cmake folder and build monica
  
  cd ~/zalf-rpm/monica
  sh update_linux.sh
  cd _cmake_linux
  make
