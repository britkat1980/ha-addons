
# Change Log

Version 3 of GivTCP is a substantial re-write with increased compatability for GivEnergy Devices and more robust modbus communication

## [3.1.3] - 2025-04-09

### Breaking Change
- Force Export now uses Discharge slot 1 to align with RTC control
- Locked down REST access to local network to enhance security
- Reboot and restart switches are now Buttons so have new device type

### Added
- RTC control for both Single and Three phase units
- Safe and "non-safe" write counts provided
- Added "Write Count" sesnor to track the daily number of register writes issued by GivTCP. This will be used in future to rate limit writes to "non write-safe" registers
- Charge Rate AC register control via REST
- Additional device type compatability in underlying library (future use)
- Prep for PV only inverters

### Fixed
- setExportTarget for EMS fixed type error
- Midnight Energy Dashboard data spike corrected
- EVC discovery index error
- Improved Subnet scanning thanks to @s0ckhamster
- Three Phase control improvements (thanks to GE for access to test kit)
- Inverter and Battery Max Power values corrected to all device types
- Updated RQ lib and associated code
- Improved auto discovery for non-standard subnet masks
- BCU temperature reporting for HV batteries
- Improved Battery (Dis)charge Rate tracking for inverters with % value only
- Removal of spurious data due to corrupt reads

## [3.0.4] - 2024-10-24
### Breaking Change
- Ability to run EVC standalone (now uses "givevc" prefix), so entity ids have changed
- Logs moved to sub-folder

### Fixed
- Data smoothing re-implemented for key Data (Recommeded to be set to Low)
- Bad data rejection, for more stable data reads
- Rapid control command stability
- REST errors due to poor chachelock implementation
- Cachelock stuck error
- Influx "None" error
- Auto discovery works with subnets bigger than /24

### Added
- Removed Node from runtime for smaller image size (Thanks Will Holley)
- Config GUI runs independent of inverters so always available

## [3.0.3] - 2024-09-25
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