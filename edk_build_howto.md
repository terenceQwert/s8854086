
1. [Build WIN32 tool]
 1.1 set path=%path%;F:\GitHub\edk_full\BaseTools\Bin\Win32
  1.2 edksetup rebuild

2. [Build EmulatorPkg]
  2.1 build
    build -t VS2019
  2.2 buil with IA32
    build -t VS2019 -a IA32
  2.3 build with X64
    build -t VS2019 -a X64

3. [Run]
got into folder EDK2\Build\EmulatorX64\DEBUG_VS2019\X64
command prompt> WinHost.exe

## Building with Pytools for OvmfPkg

**Preface**
# Python 3.8.x - Download & Install
# GIT - Download & Install
# QEMU - Download, Install, and add to your path
# Edk2 Source
# Additional packages found necessary for Ubuntu 18.04
# apt-get install gcc g++ make uuid-dev

1. <https://github.com/tianocore/edk2/blob/master/OvmfPkg/PlatformCI/ReadMe.md>

[Optional] Activate Virtual Environment - each time new shell opened

## as usual, python is 2.7, and python3 is for 3.8 are default installation on ubuntu system
## to create virtual machine on ubuntu Linux, please following command on bash
## python3 -m venv virt

**Linux**
source <name of virtual environment>/bin/activate

**Windows**
<name of virtual environment>/Scripts/activate.bat

**Intall Pytools**
generally once per virtual env or whenever pip-requirements.txt changes
--> pip install --upgrade -r pip-requirements.txt
* without these module update, below tools 'stuart_update'/'stuart_setup' tools are not existing.


**Following is just for python**

stuart_update -c OvmfPkg/PlatformCI/PlatformBuild.py TOOL_CHAIN_TAG=VS2019 -a X64
  or
stuart_update -c OvmfPkg/PlatformCI/PlatformBuild.py TOOL_CHAIN_TAG=VS2019
  ***if we define "X64" for section "TARGET_ARCH" in target.txt files alraedy, this parameter "-a" could be ignored.**
  ***if we define "VS2019" for section "TOOL_CHAIN_TAG" in target.txt files alraedy, this parameter "TOOL_CHAIN_TAG" could be ignored too.**

stuart_setup -c OvmfPkg/PlatformCI/PlatformBuild.py TOOL_CHAIN_TAG=VS2019 -a X64

python BaseTools/Edk2ToolsBuild.py -t VS2019
stuart_build -c OvmfPkg/PlatformCI/PlatformBuild.py -a X64 TOOL_CHAIN_TAG=VS2019

stuart_build -c OvmfPkg/PlatformCI/PlatformBuild.py TOOL_CHAIN_TAG=VS2019 -a X64 --FlashOnly
  or 
stuart_build -c OvmfPkg/PlatformCI/PlatformBuild.py --FlashOnly

stuart_build -c OvmfPkg/PlatformCI/PlatformBuild.py TOOL_CHAIN_TAG=VS2019 --FlashOnly

**Run & through out message (2022/3/29)**

1. refer this guidline <https://github.com/tianocore/edk2/blob/master/OvmfPkg/PlatformCI/ReadMe.md>
2. install properly version of Qemu emulator qemu-w64-setup-20210825.exe from <https://qemu.weilnetz.de/w64/2021/>
3. stuart_build -c OvmfPkg/PlatformCI/PlatformBuild.py TOOL_CHAIN_TAG=VS2019 -a X64 --FlashRom
  3.1 you can add --FlashRom to the end of your build command and the emulator will run after the build is complete
  3.2 or use the --FlashOnly feature to Just run the emulator.

**Pick up**

1. this guid 'EFI_FIRMWARE_FILE_SYSTEM2_GUID' is the first FV.



* Configure a build with edk-platforms together *
1. make sure EDK2 properly vesrioned tag download
2. configure this env variable `EDK_TOOLS_PATH` path to properly folder 
3. edksetup.bat rebuild in edk2 folder
4. visit edk-platforms\intel folder
5. type 'python build_bios.py -h', it tellls more options to build a platform BIOS
6. type 'python build_bios.py -l', it lists support platform 
7. type 'python build_bios.py -p xxx -t VS2019' to start a build with VS2019 toolchain.

** If encountering some errors that cannot be identified easy, then you can **
ex.
...
...
c:\edk_workspace\fsp;c:\edk_workspace\edk2;c:\edk_workspace;c:\edk_workspace
EDK_TOOLS_PATH   = c:\edk_workspace\edk2\basetools
CONF_PATH        = c:\edk_workspace\conf
PYTHON_COMMAND   = py -3
build: : warning: Tool chain [VS2019] is not defined


build.py...
 : error 4000: Not available
	[VS2019] not defined. No toolchain available for build!


- Failed -
Build end time: 15:10:36, Apr.25 2022
Build total time: 00:00:01


to resolve above issue, python build_bios.py --cleanall to force clean all, <-- it will cleans something files.
  