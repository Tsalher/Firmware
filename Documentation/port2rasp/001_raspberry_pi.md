# PX4 in the Raspberry Pi 3
## Compilation
### Necessary packages
Format: package (package manager)
- python-jinja2 (APT)
- python-empy (APT)
- python-pip (APT)
- catkin_pkg (pip)
- pyyaml (pip)

### Fast configuration and compilation
- Run Tools/setup/ubuntu.sh to download the necessary dependencies.
- Remove the ubuntu repositories from APT sources. There is one not found and will prevent Raspberry Pi software updates. Compilation can be achieved.
- Add -latomic to CMakeLists.txt. The std::atomic library is not added by default in ARM architectures.
- Create build directory in PX4Raspberry.
- cd to build directory and run "cmake ..".
- Run make to compile in build directory.
