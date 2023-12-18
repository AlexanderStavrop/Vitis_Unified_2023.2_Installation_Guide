# Before installation
## Update your system
```
sudo apt update -y && sudo apt upgrade -y
```
- Install needed libraries so the installation completes successfully
```
sudo apt install libncurses5-dev libncursesw5-dev libncurses5 libtinfo5 libtinfo-dev
```
<br></br>

# Downloading the installer
- <a href="https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vitis.html">Download</a>
<br></br>

# Run the installer
## Make the installer executable and run it
- Make the file executable
```
cd ~/Downloads
chmod +x <Vitis installer>
```
- Run the installer
```
./<Vitis installer>
```
<br></br>

# After the installation is completed
## Install the extra needed libraries
```
sudo ./<Vitis installation dir>Vitis/<Version>/scripts/installLibs.sh
```

## License
### Generate and download the licence
- <a href="https://www.xilinx.com/getlicense">Generate License</a>
- Move the lic file into Vitis folder
```
cp ~/Downloads/Xilinx.lic <Vitis installation folder>
```
- Open "Manage license" and link the license

## Setting up the environment
- Open bashrc for edit
```
kate ~/.bashrc
```
- Add these lines
```
# Xilinx source command
source ~/Documents/Xilinx/Vitis/2023.2/settings64.sh
export PLATFORM_REPO_PATHS="/home/alex/Documents/Xilinx/Vitis/2023.2/base_platforms/"
```
- Test by running
```
which vitis
```
```
which aiecompiler
```

## XRT
### Downlaod the latest version from git hub
```
git clone https://github.com/Xilinx/XRT.git
```
### Instal dependences
```
sudo <XRT>/src/runtime_src/tools/scripts/xrtdeps.sh
```
### Build the XRT
```
cd <XRT>/build
./build.sh
```
- If *CMAKE error occurs* change the version and build again
```
cd <XRT>/src/runtime_src
vim CMakeLists.list
```
- If *CMAKE_VERSION error occurs* remove the **first** from the following file
```
vim <XRT>/src/runtime_src/Cmakelist.txt
```
- If *PYTHON_LIBRARIES and PYTHON_INCLUDE_DIRS error occurs* add the cmake_flags line above the **-dpg** if statement
```
vim <XRT>/build/build.sh
```
```
cmake_flags+=" -DPYTHON_LIBRARY=/path/to/python/library/libpython3.x.so" # Change x to python version
cmake_flags+=" -DPYTHON_INCLUDE_DIR=/path/to/python/include"
```
- If *pybind11 error occurs* add the *export* line to bashrc
```
sudo vim ~/.bashrc
```
```
export pybind11_DIR=/usr/local/lib/python3.10/dist-packages/pybind11/share/cmake/pybind11
```
- If *openssl error occurs* comment the **find_package(OpenSSL REQUIRED)** in following CMakeLists.txt
```
vim <XRT>/src/runtime_src/tools/xclbinutil/CMakeLists.txt
```

## Sysroot
### Download the Versal common image
- <a href="https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-platforms.html">Versal common image</a>

### Extract the rootfs image
```
./sdk.sh -y -d <path_for_installation> -p
```

### Add it to Vitis
- Navigate to *File -> Preferencies -> Opne Settings(UI)*
- Search for **sysroot**
