# Files that get installed on the ENDPOINT which will forward logs TO the Splunk server

### installed during the instructions in the main folder
- inputs.conf
	- tells the Splunk Universal Forwarder what local files to send over the network to Splunk
- outputs.conf
  - tells the Forwarder where to send files (the server location)
- sysmon-config-sosalpha-JB-MODS.xml
  - the Sysmon configuration, derived from [SwiftOnSecurity](https://github.com/SwiftOnSecurity/sysmon-config)
