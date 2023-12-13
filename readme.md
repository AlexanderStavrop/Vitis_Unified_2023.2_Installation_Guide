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
```
cd ~/Downloads
chmod +x <Vitis installer>
./<Vitis installer>
```

<br></br>
# After the installation is completed
## Install the extra needed libraries
```
sudo ./<Vitis installation dir>Vitis/<Version>/scripts/installLibs.sh
```
<br></br>
## License
### Generate and download the licence
- <a href="https://www.xilinx.com/getlicense">Generate License</a>
- Move the lic file into Vitis folder
```
cp ~/Downloads/Xilinx.lic <Vitis installation folder>
```
- Open "Manage license" and link the license
<br></br>

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
- ***If CMAKE error occurs*** change the version and build again
```
cd <XRT>/src/runtime_src
vim CMakeLists.list
```
 
