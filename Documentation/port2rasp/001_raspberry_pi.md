#PX4 in the Raspberry Pi 3
## Compilation
### Necessary packages
Format: package (package manager)
- python-jinja2 (APT)
- python-empy (APT)
- python-pip (APT)
- catkin\_pkg (pip)
- pyyaml (pip)

### Fast configuration and compilation
- Run Tools/setup/ubuntu.sh to download the necessary dependencies.
- Remove the ubuntu repositories from APT sources. There is one not found and will prevent Raspberry Pi software updates. Compilation can be achieved.
- Add -latomic to CMakeLists.txt. The std::atomic library is not added by default in ARM architectures.
- Create build directory in PX4Raspberry.
- cd to build directory and run "cmake ..".
- Run make to compile in build directory.

## Wi-Fi network configuration
- Install dnsmasq (DHCP server) and hostapd (Wi-Fi network settings).
- Configure /etc/dhcpcd:
 - Create new interface "wlan0" with a fixed IP address (this will be the Rapsberry Pi's network address in wlan0).
- Configure /etc/dnsmasq.conf:
 - Set up the DHCP server (dnsmasq) in /etc/dnsmasq/dnsmasq.conf. Set up the network address range and the subnet mask. The fixed IP in /etc/dhcpcd must be inside this range.
- Configure hostapd in /etc/hostapd/hostapd.conf to set the Wi-Fi network's SSID, password, encryption algorithm and network interface.

WARNING: do not install another DHCP server, such as isc-dhcp-server. The second daemon to start will fail. DHCP IP assignment may work, but there will be a service that fails to
start due to conflict between two DHCP servers.

## Running the software
The *px4* executable requires a configuration file to run. If this file is not provided, the executable will look for the rcS configuration file in etc/. If this file is not found (which is common for regular Raspbian, the execution will fail. However, a configuration file can be specified with the `-s` option and the path to the configuration file. In the case of the Raspberry Pi 3, the execution command is the following:

`./build/px4_raspberrypi_default/px4 -s ./posix-configs/rpi/<desired_config>.config`

where `<desired_config>` is the use of the software. For example, in HITL configuration, the correct file is `px4\_hil.config`.

The executable also enables a network interface for MAVLink communications, so a USB/UART connection should not be necessary.

In order to debug directly in the Raspberry Pi, the debugger launch is similar to the run command:
`gdb ./build/px4_raspberrypi_default/px4 -s posix_configs/rpi/px4_hil.config`

If debugging remotely through an SSH connection, `gdbserver` will need to be installed in the Raspberry Pi, and the command to debug PX4 will be:

- `gdbserver <Workstation IP address>:<Port> ./build/px4_raspberrypi_default/px4 -s ./posix-configs/rpi/<desired_config>.config` in the Raspberry Pi, and
- `gdb`, then `target remote <Raspberry Pi IP address>:<Port>` within GDB's console.

Eclipse allows setting up remote debugging. First, the software must be compiled for the remote machine, and the executable must be located in both the local and the remote machine.
To debug remotely with Eclipse, add a new launch configuration for the Debug mode. A configuration window will pop up.

In the *Main* tab, C/C++ application field, the local binary's path must be specified. On the lower part of the configuration window, it is possible to configure the connection
to the Raspberry Pi. This connection can be serial, Arduino, Telnet or SSH. in this section, the absolute path to the remote executable must be specified.

In the *arguments* tab, the program can be given arguments to run. In this case, as seen above, the arguments are `-s posix_configs/rpi/<desired_config>.config`.
In the *debugger* tab, the important configuration happens in the *Gdbserver Settings* tab. This is where the port is chosen. *GDB* will be run automatically in the workstation with
the port given to *gdbserver*.
