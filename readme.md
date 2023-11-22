# Before installation
- Update your system
```
sudo apt update -y && sudo apt upgrade -y
```
- Install needed libraries so the installation completes successfully
```
sudo apt install libncurses5-dev libncursesw5-dev libncurses5 libtinfo5 libtinfo-dev
```

# Downloading the installer

- <a href="https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vitis.html">Download</a>

# Run the installer
- Make the installer executable
```
cd ~/Downloads
```
```
chmod +x <Vitis installer>
```
- Run the installation
```
./<Vitis installer>
```

# After the installation is completed
## Extra libraries
- Install the extra needed libraries
```
sudo ./<Vitis installation dir>Vitis/<Version>/scripts/installLibs.sh
```

## License
- <a href="https://www.xilinx.com/getlicense">Generate License</a>
- Download the licence and save it in the Xilinx folder
```
cp ~/Downloads/Xilinx.lic <Vitis installation folder>
```
- Open "Manage license" and link the license

## Setting up the environment
- Open bashrc for edit
```
kate ~/.bashrc
```
- Add this line
```
source <Vitis Installation path>/Vitis/<Vitis Version>/settings64.sh
```
- Test by running
```
which vitis
```
```
which aiecompiler
```
