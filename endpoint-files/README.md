# Files that get installed on the ENDPOINT which will forward logs TO the Splunk server
- A work in progress

### Updates
- 05 June 2020
	- updated to the most recent version of [SwiftOnSecurity](https://github.com/SwiftOnSecurity/sysmon-config)'s config
- 29 Feb 2020
	- added index:splunkmon/sourcetype:winnetmon to the inputs.conf file, to log all accepted inbound connections on the host
	- disabled Splunk monitoring binaries because they are very noisy

###

### installed during the instructions in the main folder
- inputs.conf
	- tells the Splunk Universal Forwarder what local files to send over the network to Splunk
	- modify the \[default\] host stanza to change the hostname (could script this, generate for each host then deploy, etc)
- outputs.conf
	- tells the Forwarder where to send files (the server location)
	- you should NOT have to configure this; just double check during the install process that it looks right
- sysmon-config-sosalpha-JB-MODS.xml
	- the Sysmon configuration, derived from [SwiftOnSecurity](https://github.com/SwiftOnSecurity/sysmon-config)

### Don't forget to make the service start automatically:
```
sc config SplunkForwarder start=auto
```
 
### Extra - enable PS logging via GPO
```
Group Policy Editor --> Computer Configuration --> Administrative Templates --> Windows Components --> Windows PowerShell
	- Module Logging --> Edit --> Enabled
		- Module Names --> Show
			- Add:  *=*
	- Turn On PowerShell Script Block Logging --> Edit --> Enabled
	- Turn On PowerShell Transcription --> Edit --> Enabled
```
