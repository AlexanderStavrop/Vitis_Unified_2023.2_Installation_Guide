# Before installation
  ## Update your system
  ```
  sudo apt update -y && sudo apt upgrade -y
  ```
  - Install needed libraries so the installation completes successfully
    ```
    sudo apt install libncurses5-dev libncursesw5-dev libncurses5 libtinfo5 libtinfo-dev -y
    ```
  <br></br>



# Instalation
  ## Downloading the installer
  - <a href="https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vitis.html">Installer</a>
  <br></br>
  
  ## Run the installer
  ### Make the installer executable and run it
  - Make the file executable
    ```
    cd ~/Downloads
    chmod +x <Vitis installer>
    ```
  - Run the installer
    ```
    sudo ./<Vitis installer>
    ```
  - Install it under /opt/xilinx/ directory
    - If there is not enough space for both Vitis and the desired device(s)
      - Install vitis with just *artix-7*
      - Launch Vivado
        ```
        cd /opt/xilinx/Installation_2023.2/Vivado/2023.2/bin
        sudo ./vivado
        ```
      - Navigate to **Help -> Add design tools or devices**
      - Install the desired device(s)
      - Delete the payload (~60 GB) (If it is not deleted by it self)
        ```
        sudo rm -rf /opt/xilinx/Installation_2023.2/Downloads/2023.2/payload
        ```
  <br></br>


# After the installation
  ## Install the extra needed libraries
  ```
  cd /opt/xilinx/Installation_2023.2/Vitis/2023.2/scripts/
  sudo ./installLibs.sh
  ```
  
  ## License
  ### Generate and download the licence
  - <a href="https://www.xilinx.com/getlicense">Generate License</a>
  - Move the lic file into Vitis folder
    ```
    sudo mv ~/Downloads/Xilinx.lic /opt/xilinx/
    ```
  - Open bashrc for edit
    ```
    vim ~/.bashrc
    ```
  - Set the bashrc variable
    ```
    # Xilinx source command
    export XILINXD_LICENSE_FILE="/opt/xilinx/Xilinx.lic"
    ```
  ## Setting up the enviromental variables
  - Open bashrc for edit
    ```
    vim ~/.bashrc
    ```
  - Add these lines
    ```
    # Xilinx source command
    source /opt/xilinx/Installation_2023.2/Vitis/2023.2/settings64.sh
    export PLATFORM_REPO_PATHS="/opt/xilinx/Installation_2023.2/Vitis/2023.2/base_platforms/"
    ```
  - Test by running (Close and re-open the terminal)
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
