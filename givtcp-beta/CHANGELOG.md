
# Change Log
All notable changes to this project will be documented in this file.

Dev branch should be used with extreme caution, mostly broken builds of WIP

Beta branch should be safe for keen users to try new features, but is not guarranteed to work.

# Change Log

All notable changes to GivTCP are documented in this file.

## [3.6.0-beta1] - 2026-10-01

Key changes since the last dev build published on the `dev3` branch (3.5.22).

Major change is migration to the published v2 (2.13.0) givenergy-modbus library be @dewet22. Outstanding work to complete the async library and incorporate the new device which had been patched here previously. This should lead to far greater reliability and stability in the code.

### Changed
- GivTCP now uses the published [givenergy-modbus](https://github.com/dewet22/givenergy-modbus) library (v2.13.0) instead of its own bundled copy. This brings more reliable modbus communication and better support for newer models. Controls the library can't write yet fail with a clear message (see [docs/upstream-givenergy-modbus-requests.md](docs/upstream-givenergy-modbus-requests.md)).
- **Hybrid HV Gen 3** inverters are now read and controlled as single-phase inverters, which is what they are, and are no longer labelled "All-in-One". Before, they were read with the three-phase layout, so values such as SOC and PV power could show as 0, and controls wrote to three-phase registers. Their Home Assistant entities change to the single-phase set, and the old three-phase ones are removed.
- **AC and All-in-One**: the PV string voltage and current entities are removed. On these models the registers don't hold real string readings (on the All-in-One the "PV voltage" was the grid voltage).
- **Set Charge Rate AC / Discharge Rate AC** now work on the All-in-One as well as the AC.
- **Faster network scan.** Inverters and EV chargers are found in one pass, scanning the host's own /24 first. A large network such as a /16 takes a few minutes rather than up to two hours, and networks larger than a /16 are limited to the /16 around the host.
- Inverter details are detected once and saved per serial number, so later start-ups skip the full detect. GivTCP detects again automatically if the saved details stop matching the inverter.
- The config page lists inverters so you can add or remove them, with no fixed limit of five. Inverters found by the last network scan are shown so you can add them in one click. Slots 1–5 keep REST ports 6345–6349, and slot 6 onwards start at 6356.
- Saving from the config page no longer resets settings the page doesn't manage back to their defaults.
- The config page now runs inside the Home Assistant sidebar (ingress) and no longer needs opening in a new tab by IP address.
- The config page works on a phone in portrait: the sections show as wrapping buttons, with a new Welcome button, and the section navigation and Previous/Save/Next buttons stay at the top while you scroll (thanks @jim-ip).
- The web pages have a new header menu (Config, Readme, Settings Guide, Datapoints, Dashboard, Logs) and restyled pages to match the config page.
- Routine messages (detecting the inverter, reconnecting, publishing discovery) are no longer logged as critical.
- Redis now only listens on localhost.

### Added
- **Log viewer** in the web UI, under Logs in the header menu. It has a tick box for each log in `/config/GivTCP/logs`: startup, each inverter's main, write and REST logs, and the EV charger logs. Tick one log to see it on its own, or any combination to merge them into one timeline, with a coloured label showing which log each entry came from. You can pick a rotated day, filter by level or text, follow new lines as they're written, load earlier lines and download a log file. Only the log files are served: nothing else in `/config/GivTCP`, such as `allsettings.json`, can be reached through the viewer.
- **Time picker controls for timeslots** in Home Assistant 2026.5 or later, alongside the existing drop-downs. For example, "Charge start slot 1" sits next to "Charge start time slot 1". Both stay in sync.
- **Settings Guide and Datapoints pages** in the web UI, generated from [docs/SETTINGS-GUIDE.md](docs/SETTINGS-GUIDE.md) and the new [docs/DATAPOINTS.md](docs/DATAPOINTS.md), which describes every value GivTCP publishes.
- **REST request log**: each REST request and its result are written to their own log file alongside the main log. Settings requests are never logged in full, as they can contain passwords, and full data dumps are logged by size only.
- **Battery Pause Mode and the pause timeslot on Gen 1 (firmware 187 or later) and Gen 2 hybrids.** givenergy-modbus doesn't support them on hybrids yet, so GivTCP adds them itself until it does.
- **Per-slot charge and discharge target SOC** on inverters with 10 time slots (newer Gen 3, Gen 4, All-in-One, HV Gen 3) and on three-phase inverters, where charge/discharge slots 3-10 are now also read, **charge and discharge rate on three-phase inverters**, and **Car Charge Boost on the EMS**. givenergy-modbus doesn't support these yet, so GivTCP adds them itself until it does.
- **Data age stat** (`Data_Age`): shows how old the inverter data is, so values held from the last good read can be spotted.
- **Battery charge and discharge MOS state** entities, showing whether each battery's charge and discharge switches are open or closed: for each HV battery stack and each LV battery (thanks @plandregan).
- Better support for HV Gen 3, three-phase, All-in-One, EMS and Gateway systems, including battery counts and stack details for HV batteries.

### Fixed
- **Gateway controls** all failed with an error such as `'GatewayV1' object has no attribute 'set_enable_discharge'`. They now work.
- **Inverter data failed to process**, and nothing was published, when some holding registers couldn't be read: for example the Gateway's charge/discharge limits, the battery reserve, charge target or a timeslot, or the inverter clock. The last good values are used instead.
- If processing a poll fails for any other reason, the last good data is republished instead of entities going unavailable.
- **Today energy stats stuck at midnight.** Load and Self Consumption Today, and the Day/Night cost stats, could keep yesterday's value if no poll landed in the exact minute of midnight. They now reset when the inverter's date changes, even after a missed poll, a restart over midnight or clock drift. The midnight "zero" message, which could double-count in the HA Energy dashboard, has been removed.
- **Eco (Paused)** battery mode, and reverting a Force Charge on three-phase inverters, failed with `CancelledError`. A command that wrote the same register twice cancelled its own first write; now only the last write to each register is sent.
- **Force Export** put your discharge slot 1 times into charge slot 1 when it ended, and left discharge slot 1 set to the export window. It now restores discharge slot 1.
- **Export (Paused)** could be chosen as the battery mode but was rejected, so ending a Force Export started from that mode also failed.
- **Force Charge / Force Export** didn't revert when some of the settings they save couldn't be read. They now restore the settings that were read.
- **Cancelling Force Charge / Force Export** from REST or MQTT failed, and dropped any other pending controls.
- **Cancelling Temp Pause Charge / Discharge** from REST returned a server error.
- **Enable Discharge** (REST and MQTT) did nothing. It now sets the battery reserve: back to the saved reserve to enable, or to 100% to disable.
- **Set Date and Time** always failed.
- **Set PV Input Mode** was silently ignored. It now reports that givenergy-modbus can't write it yet.
- **Car Charge Boost over MQTT** used the wrong payload key.
- **EMS discharge target SOC** can now be set over MQTT.
- **Three-phase Force Charge, Force Discharge and AC Charge** can now be set over REST.
- Several controls could fail with an error that dropped every other pending control and left a REST caller waiting for a timeout: Set Charge/Discharge Rate and Set Eco Mode on EMS, and Force Charge/Export before any inverter data had been read. They now fail with a clear message.
- Force Charge, Force Export and Temp Pause failed on models whose data didn't include the settings they save for reverting (three-phase battery reserve, EMS charge rate).
- A control that isn't available on your inverter model now says so ("not available for Ems inverters"), rather than giving an `AttributeError`. This covers, for example, three-phase-only controls on single-phase inverters, inverter controls on the EMS, and Force Charge/Export on the EMS.
- A control sent at the same moment as the read loop checked for requests could be lost, along with any other pending requests.
- **Leftover pause entities on older inverters.** Battery Pause Mode and the pause timeslots are now removed from Home Assistant on inverters that don't support them (Gen 1 hybrid & AC on old f/w).
- Gen 1 Home Assistant discovery failed on the battery BMS current entity, so no entities were created.
- **Export Power Limit** now shows in Watts in HA, instead of as an amp slider.
- **Battery pause slot changes** now show in HA immediately, instead of after the next full read.
- The Settings Guide page opened the settings API instead of the guide.
- The config page could load blank in Chrome after an update, because the browser kept an old copy of the page that pointed at files that no longer exist. The page is no longer cached (thanks @jim-ip).
- The Smart Target setting was ignored and followed the Dynamic Tariff setting instead (thanks @jim-ip).
- Charge rate on large battery banks no longer jumps to 50% at the inverter maximum.
- The limits used to filter bad readings are higher, so large systems aren't wrongly rejected: 20 kW battery power for parallel All-in-Ones, 100 kWh/day for homes with heat pumps and EVs.
- Unused slots with a 0% target are no longer published, which stops repeated HA errors.
- If InfluxDB is unavailable, polling carries on instead of stalling for minutes.

### Removed
- Lite query mode and its settings.


## [3.3.15] - 2025-10-21
### Fixed
- Improved connection handling to remove timeout errors
- Addition of Gen4 and Gen3+ model types
- Hybrid HV Gen3 compatability
- Load Energy Today data stability
- refined start-up inverter discovery
- RTC state error

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