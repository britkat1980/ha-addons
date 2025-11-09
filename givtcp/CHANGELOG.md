
# Change Log

Version 3 of GivTCP is a substantial re-write with increased compatability for GivEnergy Devices and more robust modbus communication

## [3.5] - 2025-11-09
### Fixed
- Mqtt discovery fix for object_id to use new default_entity_id
- improved error handling for timeslots with minute value of 60 instead of 0
- Improved modbus connection handling
- Load Energy resets at Midnight now stable
- Fix for HV Gen3 comms errors

## [3.4.1] - 2025-10-22
### Fixed
- state_class errors for some entities

## [3.4] - 2025-10-22
### Fixed
- Rolled back mqtt discovery fix for object_id to resolve "unnamed_device"
- Fix for missing unit_of_measurement for battery capacity- Fix for isoformat error associated with REST calls

## [3.3] - 2025-10-20
### Fixed
- Improved connection handling to remove timeout errors
- Addition of Gen4 and Gen3+ model types
- refined start-up inverter discovery
- RTC state error


## [3.2] - 2025-08-26
### Fixed
- Solarmode for EVC
- RTC control stauts stability fix
- Refactored inverter Model to include new/upcoming models

### Added
- Web Dashboard updated
- Meter Data now available for up to 8 meters (subject to firmware availability)

## [3.1.6] - 2025-04-13
### Breaking Change
- Battery Calibration is now split into two entities, one sensor for status plus a select control to control calibration modes. This fixes the issue on failing to start GivTCP during a calibration

### Added
- Updated Web Dashboard to latest version (Thanks @DanGallo)

### Fixed
- Single AIO Gateway error. Includes log line to recommend turning off Gateway in config for a single Gateway/AIO system

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