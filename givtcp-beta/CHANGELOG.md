
# Change Log
All notable changes to this project will be documented in this file.

Dev branch should be used with extreme caution, mostly broken builds of WIP

Beta branch should be safe for keen users to try new features, but is not guarranteed to work.

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