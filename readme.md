# Vitis Unified 2023.2 Installation guide
This guide is intended to help you install Vitis 2023.2 and the necessary libraries to create a system project for the Versal Vck190 platform.



# Table of Contents
  1. [Prerequisites](#Prerequisites)
  2. [Installation](#Installation)
  3. [Post installation](#Post-installation)
  4. [Xilinx Runtime Library (XRT)](#Xilinx-Runtime-Library(XRT))
  5. Rootfs-Kernel Image-Sysroot

## Prerequisites
Installing *Vitis* for the *Versal AI Core Series* typically requires approximately **260GB** of space on the host machine, with post-installation space reduced to approximately **160GB**. However, we can tailor the Installation process to fit within the confines of a 256GB SSD. This guide will walk you through the process using *Ubuntu 22.04 LTS* as the Linux operating system required for XRT library compatibility.

### Update the host system and install the required libraries
- Update and upgrade the host system
  ```Bash
  sudo apt update -y a sudo apt upgrade -y
  ```
- Install the required libraries (otherwise the installation will nat complete successfully)
  ```Bash
  sudo apt install Linncurses5-dev Liöncursesw-dev liüncursess Libtinfus libtinfo-dev -y
  ```


## Installation
Once the necessary requirements have been met, the installation can begin.

### Downloading the installer
Download the  <a href="https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vitis.html">Linux Self Extracting Web Installer</a> (This installation guide is for version 2023.2. Newer versions may differ)

### Run the installer
- Make the installer executable
  ```Bash
  cd ~/Download
  ```
  ```Bash
  chmod +x <Vitis installer>
  ```

### Run the installer
- Make the installer executable
    ```bash
    cd ~/Downloads
    ```
    ```bash
    chmod +x <Vitis installer>
    ```
- Run the installer
    ```bash
    sudo ./<Vitis installer>
    ```
    - Login  
    - Select *Vitis* as the product to install  
    - Under *Devices > Device custom platforms* check **only** *Versal ACAP > Versal AI Core Series*
       - If there is not enough space for both Vitis and the Versal AI Core platform check **only** *7 Series > Artix-7*
    - Agree to everything  
    - It is recommended that you install Vitis in the *opt/xilinx* directory.

- If you are installing on a 256GB ssd, after installation completes, 
  - Launch Vivado
      ```Bash
      cd /opt/xilinx/Vivado/2023.2/bin
      ```
      ```bash
      sudo ./vivado
      ```
  - Navigate to *Help -> Add design tools or devices* and select *Versal AI Core Series*
  - Delete the payload (~60 GB) (If it is not deleted by it self)
    ```Bash
    sudo rm -rf /opt/xilinx/Installation_2023.2/Downloads/2023.2/payload
    ```


## Post installation
After completing the installation, you'll need to proceed with installing necessary libraries and associating your license.

### Install the extra needed libraries
  ```Bash
  cd /opt/xilinx/Installation_2023.2/Vitis/2023.2/scripts/
  ```
  ```Bash
  sudo ./installLibs.sh
  ```
### License
- If you haven't already generated a licence, use <a href="https://www.xilinx.com/getlicense">this link</a> to generate one
- Download the *.lic* file and move it to the opt directory
  ```Bash
  sudo mv ~/Downloads/Xilinx.lic /opt/xilinx/
  ```
- Edit the bashrc file and export the *XILINXD_LICENSE_FILE* variable
  ```Bash
  vim ~/.bashrc
  ```
  ```Bash
  # Xilinx source command
  export XILINXD_LICENSE_FILE="/opt/xilinx/Xilinx.lic"
  ```
- Test by running after closing and re-openning the terminal
    ```
    which vitis
    ```
    ```
    which aiecompiler
    ```
  - The location of *Vitis* and *aiecompiler* should be printed with each command.


  ## Xilinx Runtime Library(XRT)
  ### Check the cmake version
  ```Bash
  cmake --version
  ```
  - If the version has a single digit, move the cmake version from vitis to desktop
    ```
    sudo mv /opt/xilinx/Installation_2023.2/Vitis/2023.2/tps/lnx64/cmake-3.3.2/ ~/Desktop/
    ```
  - Exit the terminal, reopen it and check the version again
    ```
    cmake --version
    ```
  ### Downlaod the latest version from git hub
  ```
  cd ~/Downloads
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
  - If the libtinfo.so.11 library cannot be found
    ```
    sudo apt update && sudo apt upgrade -y
    cd ~/Downloads
    wget http://mirrors.kernel.org/ubuntu/pool/main/libi/libidn/libidn11_1.33-2.2ubuntu2_amd64.deb
    sudo apt install ./libidn11_1.33-2.2ubuntu2_amd64.deb
    rm libidn11_1.33-2.2ubuntu2_amd64.deb
    ```
  - Try building it again
    ```
    cd XRT/build/
    ./build.sh
    ```
  ### Build the Rersources
  ```
  cd Release
  make package
  ```
  - Move the xrt folder inside the opt directory
    ```
    cd opt/xilinx
    sudo mv -r xrt/ /opt/xilinx
    ```
  - Delete the xrt directory
    ```
    sudo rm -r ~/Downloads/XRT/
    ```
  - Move the cmake back to Vitis directory
    ```
    sudo mv ~/Desktop/cmake-3.3.2 /opt/xilinx/Installation_2023.2/Vitis/2023.2/tps/lnx64/
    ```
  ### Set the enviromental variable
  - Open bashrc for edit
    ```
    vim ~/.bashrc
    ```
  - Add the following line
    ```
    export XILINX_XRT="/opt/xilinx/xrt/"
    ```
  ### Include the xrt library in Vitis
  - Open Vitis and navigate to your ***Application*** component
  - In ***Settings*** open the ***userConfig.cmake***
  - In ***Directories*** browse and add the ***xrt*** directory

  ## Kernel Image - Rootfs - Sysroot
  ### Download the Versal common image
  - <a href="https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-platforms.html">Versal common image</a>
  
  ### Extract the rootfs image
  ```
  cd ~/Downloads
  tar -xzvf xilinx-versal-common-v2023.2_10140544.tar.gz -C /opt/xilinx/
  ```
  ### Extracting the SDK
  ```
  cd /opt/xilinx/xilinx-versal-common-v2023.2
  ./sdk.sh -y -d /opt/xilinx/xilinx-versal-common-v2023.2/ -p
  ```
  ### Index the corresponding file to Vitis
  #### Kernel Image
  - Navigate to *File -> Preferencies -> Open Settings(UI)*
  - Search for **sysroot**
  - Lind the **Cortexa72-cortexa53-xilinx-linux** directory
  <br></br>

# Building a project
- If *asm library* cannot be found
  ```
  sudo ln -s /usr/include/asm-generic/ /usr/include/asm
  ```
- If *wordsize library* cannot be found
  ```
  sudo apt install gcc-multilib
  ```


  # DONT FORGET THE FUCKING AI ENGINE PACKAGE DO NOT START THE GRAPH :(
