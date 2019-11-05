## PX4Raspberry
This project's goal is to be able to run the PX4 software in a Raspberry Pi.

The autopilot logic is the same as for the Pixhawk hardware, but the hardware, the OS and the interfaces change. The Raspberry Pi 3 has four USB ports and
one Fast Ethernet port for data transfer. Raspbian is not a RTOS (Real-Time Operating System), so changes may need to be made in case of trouble with the
instructions' execution order.

# Interesting directories in the source code:
 - systemcmds
 - modules/px4iofirmware
 - modules/sensors
 - platforms
 - boards/px4/raspberrypi
 - src/drivers/rpi_rc_in

# Raspberry Pi 3 reference:
 - [GPIO pins](https://www.raspberrypi.org/documentation/usage/gpio/)
 - [Interfacing with GPIO pins](https://kernel.googlesource.com/pub/scm/libs/libgpiod/libgpiod/+/v0.2.x/README.md)

# To compile:
 - Install python-jinja2 (name for Ubuntu repositories).
