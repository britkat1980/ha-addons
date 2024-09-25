
# Change Log

Version 3 of GivTCP is a substantial re-write with increased compatability for GivEnergy Devices and more robust modbus communication

## [3.0.2] - 2024-09-25
### Fixed
- Startup error when reported network is blank

## [3.0.1] - 2024-09-25
### Fixed
- Missing HostIP gracefully handled and added as config option
- Turned off EVC by default
- Removed datasmoothing from battery data to stop "sticky data"
- Gateway eco-mode and battery calibration control fixed
- FindInverter updated to account for /23 subnet mask
- stability updates for switch devices
- Improved upgrade logic to remove old MQTT discovery topics
- Updated base image to Alpine 3.19 and Python 3.12.6


## [3.0.0] - 2024-09-14
### Added
- New asynchronus library providing more robust communicaition
- Compatability with ALL Givenergy device types inc: Three Phase, EMS and Gateway
- Auto discovery with (near) zero-config setup. Device discovery and feature detection (firmware/batteries etc...)
- Web Based config
- Internal watchdog to restart GivTCP in the event of process hangs
- Two stage timer loop to minimise unnecessary modbus calls
- Full HV battery stack compatability
- Long-lived network connections for modbus and MQTT for greater stability
- Auto update of device IP address, incase your inverter is not on a static IP
- Numerous under the hood tweaks for faster and more reliable control response times.