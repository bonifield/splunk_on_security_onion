# Files that get installed on the ENDPOINT which will forward logs TO the Splunk server

### installed during the instructions in the main folder
- inputs.conf
	- tells the Splunk Universal Forwarder what local files to send over the network to Splunk
	- modify the \[default\] host stanza to change the hostname (could script this, generate for each host then deploy, etc)
- outputs.conf
  - tells the Forwarder where to send files (the server location)
  - you should NOT have to configure this; just double check during the install process that it looks right
- sysmon-config-sosalpha-JB-MODS.xml
  - the Sysmon configuration, derived from [SwiftOnSecurity](https://github.com/SwiftOnSecurity/sysmon-config)
