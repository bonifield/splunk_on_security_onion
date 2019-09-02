# Files that get installed on the SPLUNK SERVER

### installed during the instructions in the main folder
- inputs.conf
	- tells Splunk what local files and folders to monitor, in this case reading in JSON-formatted Zeek (Bro) logs
- props.conf
	- handles sourcetype and field formatting, and field reporting ("send this field with this transforms.conf formatting to Splunk)
- transforms.conf
	- handles field extractions and transforms (re-arranging, etc)

### optionally installed through the Splunk web GUI
- conn_state_table.csv
	- a [lookup table](https://docs.splunk.com/Documentation/Splunk/7.3.1/Knowledge/DefineanautomaticlookupinSplunkWeb) for Zeek's conn_state field
- EventCode_table.csv
	- a lookup table for Sysmon event codes
