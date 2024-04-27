
# Change Log
All notable changes to this project will be documented in this file.

Dev brnach should be used with extreme caution, mostly broken builds of WIP

Beta branch should be safe for keen users to try new features, but is not guarranteed to work.

## [2.4.155] - 2024-04-27

Multi inverter set-ups currently unstable. Feedback needed.

### Added
- Uses new async library for more reliable modbus comms
- Auto retries 5 time for every command
- AIO battery data now available
- AC battery power output % control
- network scanning now more robust (longer timeout)
- Full and Partial refresh of data. There are two "self run" timers in the config. One for frequently changing power data etc...(defaul 30s) one for everything else (default 120s)