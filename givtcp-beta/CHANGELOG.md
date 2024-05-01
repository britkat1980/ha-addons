
# Change Log
All notable changes to this project will be documented in this file.

Dev branch should be used with extreme caution, mostly broken builds of WIP

Beta branch should be safe for keen users to try new features, but is not guarranteed to work.

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