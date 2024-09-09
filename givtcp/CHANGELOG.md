
# Change Log

## [2.4.8] - 2024-09-09
### Fixed
- Pre v3 release, will store v2 settings locally for use with v3 upgrade

# =========== COMING SOON ==========

Version 3 of GivTCP is a substantial re-write with increased compatability for GivEnergy Devices and more robust modbus communication

## [3.0.0] - 2024-08-24
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