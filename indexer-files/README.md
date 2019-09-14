# Files that get installed on the SPLUNK INDEXER (Security Onion)

## installed during the instructions in the main folder
- inputs.conf
	- tells Splunk what local files and folders to monitor, in this case reading in JSON-formatted Zeek (Bro) logs
- props.conf
	- handles sourcetype and field formatting, field aliases (alternate ways to call a field), and field reporting (aka "send this field with this transforms.conf formatting to Splunk")
- transforms.conf
	- handles field extractions and transforms (re-arranging, etc)
	- portions of the Sysmon section derived from this [Splunk blog](https://www.splunk.com/blog/2014/11/24/monitoring-network-traffic-with-sysmon-and-splunk.html)

## optionally installed through the Splunk web GUI
- conn_state_table.csv
	- a [lookup table](https://docs.splunk.com/Documentation/Splunk/7.3.1/Knowledge/DefineanautomaticlookupinSplunkWeb) for Zeek's conn_state field
- EventCode_table.csv
	- a lookup table for Sysmon event codes

## custom field extractions

### notes about field meanings
- Image = official term for a compiled binary file
- Subject_* = the one performing the action in question; the account requesting a logon (service, user, etc) but NOT the actual account logging on
- Target_* and User_* = the one being acted upon by the Subject
- New_Logon_* = the account for whom the new logon was created, i.e. the account logged on

### Windows Event Logs - Security
- Message_Title
	- the first summarizing sentence in the log describing the event
- Process_ID_Number
	- int value
- Subject_Security_ID
- Subject_Account_Name
- Subject_Account_Domain
- Subject_Logon_ID
	- hex value
- Subject_Account_Logon_ID_Number
	- int value
- Target_Security_ID
- Target_Account_Name
- Target_Account_Domain
- User_Security_ID
- User_Account_Name
- User_Account_Domain
- New_Account_Security_ID
	- also included new Computer accounts
- New_Account_Name
	- also included new Computer accounts
- New_Account_Domain
	- also included new Computer accounts
- New_Logon_Security_ID
- New_Logon_Account_Name
- New_Logon_Account_Domain
- New_Logon_ID
	- hex value
- New_Logon_ID_Number
	- int value
- New_Logon_Linked_ID
	- hex value
- New_Logon_Linked_ID_Number
	- int value
- New_Logon_GUID
- Group_Security_ID
- Group_Name
- Group_Domain
- Locked_Security_ID
- Locked_Account_Name

### Windows Event Logs - PowerShell
- ScriptBlock
- ScriptBlockID
