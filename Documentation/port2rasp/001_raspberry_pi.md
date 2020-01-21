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

## Wi-Fi network configuration
- Install dnsmasq (DHCP server) and hostapd (Wi-Fi network settings).
- Configure /etc/dhcpcd:
 - Create new interface "wlan0" with a fixed IP address (this will be the Rapsberry Pi's network address in wlan0).
- Configure /etc/dnsmasq.conf:
 - Set up the DHCP server (dnsmasq) in /etc/dnsmasq/dnsmasq.conf. Set up the network address range and the subnet mask. The fixed IP in /etc/dhcpcd must be inside this range.
- Configure hostapd in /etc/hostapd/hostapd.conf to set the Wi-Fi network's SSID, password, encryption algorithm and network interface.

WARNING: do not install another DHCP server, such as isc-dhcp-server. The second daemon to start will fail. DHCP IP assignment may work, but there will be a service that fails to
start due to conflict between two DHCP servers.
