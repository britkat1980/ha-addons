
# Change Log
All notable changes to this project will be documented in this file.

Dev branch should be used with extreme caution, mostly broken builds of WIP

Beta branch should be safe for keen users to try new features, but is not guarranteed to work.

## [3.1.8] - 2025-05-02
### Fixed
- Solarmode for EVC
- RTC control staus stability fix
- Refatored inverter Model to include new/upcoming models

### Added
- Web Dashboard updated
- Meter Data now available for up to 8 meters (subject to firmware availability)

## [3.1.4] - 2025-04-09
### Breaking Change
- Reverting Battery Calibration to Select control, but retaining seperate sensor 

### Added
- Updated Web Dashboard to latest version (Thanks @DanGallo)

## [3.1.4] - 2025-04-09
### Breaking Change
- Battery Calibration is now split into two entities, one sensor for status plus a switch to turn on or off calibration. This fixes the issue on failing to start GivTCP during a calibration

### Fixed
- Single AIO Gateway error. Includes log line to recommend turning off Gateway in config for a single Gateway/AIO system

## [3.1.3] - 2025-04-08
### Fixed
- Improved Subnet scanning thanks to @s0ckhamster

## [3.1.2] - 2025-03-03
### Breaking Change
- Force Export now uses Discharge slot 1 to align with RTC control
- Locked down REST access to local network to enhance security

### Added
- RTC control for both Single and Three phase units
- Safe and "non-safe" write counts provided

### Fixed
- setExportTarget for EMS fixed type error
- EVC discovery index error

## [3.1.1] - 2025-02-10
### Fixed
- Three Phase control improvements (thanks to GE for access to test kit)
- Inverter and Battery Max Power values corrected to all device types
- Updated RQ lib and associated code
- Improved auto discovery for non-standard subnet masks
- BCU temperature reporting for HV batteries
- Improved Battery (Dis)charge Rate tracking for inverters with % value only
- Removal of spurious data due to corrupt reads


### Added
- Added "Write Count" sesnor to track the daily number of register writes issued by GivTCP. This will be used in future to rate limit writes to "non write-safe" registers
- Charge Rate AC register control via REST
- Additional device type compatability in underlying library (future use)
- Prep for PV only inverters



## [3.0.4] - 2024-10-22
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

## [3.0.1] - 2024-09-18
### Fixed
- Missing HostIP gracefully handled and added as config option
- Turned off EVC by default
- removed datasmoothing from battery data to stop "sticky data"
- Gateway eco-mode and battery calibration control fixed

## [3.0.0i] - 2024-09-14
### Fixed
- Fix for sporadic Battery Key error

## [3.0.0h] - 2024-09-13
### Fixed
- Fixed EVC discovery error
- Improved v2 upgrade logic

## [3.0.0g] - 2024-09-12
### Fixed
- Today Energy not resetting at midnight

## [3.0.0f] - 2024-09-09
### Fixed
- v2env.pkl location error

## [3.0.0e] - 2024-09-09
### Fixed
- 0% SOC drops
- Midnight Energy Errors
- Import_ppkwh missing error

## [3.0.0.d] - 2024-09-07
### Fixed
- Single AIO SOC error
- Gateway Battery Power

## [3.0.0.c] - 2024-08-01
### Fixed
- Three Phase Charge Schedules

## [3.0.0.b] - 2024-08-01
### Fixed
- setChargeSlot type error fixed
- Single Gateway/AIO SOC error fixed

## [3.0.0.a] - 2024-08-01
### Added
- Final v3 Beta


## [2.4.663] - 2024-08-01
### Fixed
- ForceCharge fix for Three Phase and error stuck on Eco(Paused)
### Added
- Force Charge, Force Discharge and AC Enable controls added for Three Phase

## [2.4.652] - 2024-08-01
### Fixed
- ForceCharge and ForceExport now us the refactored threephase control

## [2.4.649] - 2024-08-01
### Fixed
- Three phase timeslots and charge targets refactored

## [2.4.642] - 2024-08-01
### Fixed
- Self_run loop restart fixed with longer timeout before restart

## [2.4.614] - 2024-08-01
### Fixed
- Config flow tweaks

## [2.4.611] - 2024-08-01
### Fixed
- MQTT connection instability
- Three phase read keyerror
- EVC importcap running only when charging

## [2.4.589] - 2024-07-30
### Fixed
- NUMINVERTERS type error

## [2.4.588] - 2024-07-30
### Fixed
- Improved auto discovery for EVC

## [2.4.544] - 2024-05-14
### Fixed
- improved REST responses
- ignore blank IP addresses in HostIP

## [2.4.513] - 2024-05-14

### Added
- Web GUI config
- Re-architected write commands to use persistent modbus connection
- Added ECO toggle to REST

## [2.4.265] - 2024-05-14

### Fixed
- Corrected Gateway and HV battery software version

### Added
- Ability to retrieve meter data, as polled by the portal. Provides 5min interval data (in "raw" mqtt output). Specifically useful for those with additional meters for heat pumps etc... 

## [2.4.252] - 2024-05-14

### Fixed
- Corrected Energy scaling factor for Three Phase inverters

### Added
- Direct Meter data in "raw" output on MQTT

## [2.4.252] - 2024-05-14

### Fixed
- Spelling change on RAW output

### Added
- Battery Calibration control

## [2.4.248] - 2024-05-05

Note that there is a new GivTCP Stats device created where things like Last Updated Time and other stats appear, should be same identity ID in HA

### Added
- Gateway first draft (read plus chrge target control)
- EMS first draft (read plus timeslot control)
- Three phase first draft (read only)


### Fixed
- Predbat compatability improvement
- Proper tracking of missed read calls, via "Time Since Last Update" entity

## [2.4.195] - 2024-05-01

### Fixed
- fixed compatability with "old firmware" models
- auto update cache after write (for REST/predbat)

## [2.4.193] - 2024-05-01

Multi inverter set-ups currently unstable. Feedback needed.

### Fixed
- Write command response accurately returns an error
- REST command updated to avoid conflicts with async
- predbat compatability now under test (should work)
- tweaks to findinverter flow ready for zero-conf imlementation
- prepwork for EMS and 3-three-phase inverters

## [2.4.155] - 2024-04-27

Multi inverter set-ups currently unstable. Feedback needed.

### Added
- Uses new async library for more reliable modbus comms
- Auto retries 5 time for every command
- AIO battery data now available
- AC battery power output % control
- network scanning now more robust (longer timeout)
- Full and Partial refresh of data. There are two "self run" timers in the config. One for frequently changing power data etc...(defaul 30s) one for everything else (default 120s)