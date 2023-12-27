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
### Check the cmake version
```
cmake --version
```
- If the version has a single digit, move the cmake version from vitis to desktop
```
mv <Vitis>/<Version>/tps/lnx64/cmake-3.3.2 ~/Desktop/
```
- Exit the terminal, reopen it and check the version again
```
cmake --version
```

### Downlaod the latest version from git hub
```
git clone https://github.com/Xilinx/XRT.git
```
### Instal dependences
```
cd XRT
sudo src/runtime_src/tools/scripts/xrtdeps.sh
```
### Build the XRT
```
cd build
./build.sh
```
### Build the Rersources
```
cd Release
make package
```
- Move the cmake back to Vitis directory
```
mv ~/Desktop/cmake-3.3.2 <Vitis>/<Version>/tps/lnx64/
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
