Gathering resources and ideas for the D-Studio RFID interlock system
## Goal
Create a custom power interlock system for use in the UDEL Pit that controls access using the RFID functionality of our UDIDs
## Requirements
1) Uniquely identifies the user via RFID
2) Can switch 120v and 220v single phase devices on and off
3) Integrates safety functionality such as pressure mats and light curtains (Expandable)
4) Provides a simple interface to enroll new users and update info in the access control system
5) Built with off the shelf components
6) Well documented to enable building more

## High Level Architecture

**ESP32**
- Reads RFID cards
- Checks safety interlocks
- Stores a copy of the ACL (Access control List) 
- Checks the UDID against the ACL
- Listens to MQTT to receive direct commands
- Sends requests to the webserver to download a new copy of the ACL when required


**RPI**
*Runs ACL webserver*
- Provides API for interlocks to download copies of the ACL
- Provides API for logging functionality
- Provides API for registering nodes on mqtt

*Runs Interface*
- Provides interface to update database
- Provides interface to add new users to database
- Provides interface to initiate sync

*Runs MQTT Network*
- Sends ACL update messages to interlocks
- Sends all unlock requests to interlocks
- Sends all lock requests to interlocks
- Gets node data from interlocks

## Implementation

- ACL as Sqlite3 database on ESP32 SD card
- During operation, UID is obtained and checked against ACL by key locally on esp32
- If user has permissions in ACL and interlocks are satisfied, the power is switched on and event is logged with an http post request
- On mqtt request to update, ESP32 downloads ACL database via http get request and overwrites the copy stored on the SD card
- On mqtt message to unlock all, ESP32 switches on power, ignoring interlocks (Or maybe just disregardes access permissions)

**Next Steps**
Define what the access permission system looks like
- Is it one identifier per machine?
- Is it stratified "Mastery Levels"?