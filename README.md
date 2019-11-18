# Splunk on Security Onion

### Objectives:
- Manage logs with Splunk, versus the ELK stack built into Security Onion
- Ingest Windows Application, Security, System, and Sysmon logs via forwarding from remote hosts into Splunk on Security Onion
- Centrally monitor home or small office networks, especially for the purposes of home lab penetration testing and post-analysis
- Provide all necessary configuration files, and all necessary commands in a streamlined set of instructions

### What This IS:
- A manual, but streamlined, method of installing and configuring Splunk on a standalone instance of Security Onion to receive Zeek, Sysmon, and Windows Event logs (Application, Security, System)
	- A series of very clear configuration files, and instructions on where to put them
	- A series of instructions that work across Debian-based Linux builds (Debian, Ubuntu, Security Onion, etc)
- A fully functional project (note points below as to what is not yet included)
- A pet project
- A work in progress


### What This IS NOT:
- A perfect solution
- ~~A solution that uses intermediate forwarders; this is one of many lab setups, though intermediate forwarders would greatly enhance the function, and security, of this project~~ Now, with Intermediate Forwarder configs!
- Splunk Dashboards 101 (though a basic one is included in my helpers folder)
- An application (sure, apps are easy, but they don't show me how or why they work)
- A finished project (Snort and OSSEC parsers aren't included, nor even finished, as of this writing)
- A Splunk license (use the free one for your home lab)
- A series of instructions for RHEL/CentOS/Fedora/etc.  This is for Security Onion specifically, but it works just as well (identically) on Ubuntu.

### Considerations:
- The amount of storage you have available
	- Do you really need full PCAP for everything, or can you shorten how long files are saved?
- The size of the network you are collecting from
	- Does your Security Onion have enough resources?
		- Zeek and Splunk may need to be tuned individually
	- Can you transport all of those logs across the network without impacting your business operations?
- Log data relevance
	- The Sysmon configuration here is very wide open, moreso than the SwiftOnSecurity alpha config.
		- Do you want to make a wide-open anything-goes config?  Do you want even more restrictions?
		- Do you *need* specific logs, i.e. Application, certain Zeek logs, etc?

### If using Ubuntu and not Security Onion, this requires that Zeek (Bro) produce logs in JSON format, and the monitor paths be changed as necessary in the server's inputs.conf file.  Otherwise, all configurations should work exactly the same.

### TODO
- Field modifications:  CEF compliance, better extractions (index-time and search-time), MORE extractions/fixes as needed (there are definitely some I missed)
- Create scripts and/or GPOs to install the forwarders across a network
- Actually utilize Splunk's ability to create new (non-default) certificates when deploying to hosts
- Add hardening guide (unlikely)
- ~~Intermediate forwarders (because the whole network does NOT need direct access to Security Onion...)~~
- Repo for Win/Sysmon on ELK, the exact reverse of this project
- Snort and OSSEC parsers
- Custom Python scripts that enrich the indexes, provide lookups, API calls, etc 
- Visual map of how these configs interact with each other
- Automate and/or containerize

## INSTRUCTIONS

### On Security Onion
- Install Security Onion, DO NOT ENABLE ELK WHEN PROMPTED DURING THE THIRD SETUP PHASE
  - Make sure your storage capabilities are pretty solid, or just turn off full PCAP if it's not important to you
- Install Splunk
- Configure Splunk to start on boot (Ubuntu/systemd, other systems read [here](https://docs.splunk.com/Documentation/Splunk/8.0.0/Admin/ConfigureSplunktostartatboottime)
```
sudo dpkg -i [yoursplunkfile].deb
sudo /opt/splunk/bin/splunk start
sudo /opt/splunk/bin/splunk enable boot-start -systemd-managed 1
```
- Log into the Splunk GUI, and configure the server to use HTTPS
```
Settings --> Server Settings --> General Settings --> Enable SSL (HTTPS) in Splunk Web? --> YES
```
- Log back into Splunk and create three new indexes: bro, sysmon, and winevt
	- Change other options as desired, but it's extremely unlikely you won't need to for a home/small business setting
```
Settings --> Indexes --> New Index --> Index Name:  bro --> Save
                         New Index --> Index Name:  sysmon --> Save
                         New Index --> Index Name:  winevt --> Save
```
- Make the new indexes viewable on the homepage Data Summary and for any user account deemed necessary
```
Settings --> Access Controls --> Roles --> [role] --> Indexes tab --> select the checkboxes in both Included and Default for desired index
```
- Add a Receiving Port for Splunk to ingest logs from other hosts
```
Splunk --> Settings --> Forwarding and receiving --> Configure receiving --> + Add New --> 9997 --> Save
```
- Configure UFW (firewall) to allow forwarded logs (from endpoints or Intermediate Forwarder, see below) to the Receiver
```
sudo ufw allow 9997/tcp
OR
sudo ufw allow proto tcp from [host-ip] to [indexer-ip] port 9997
```
- Place inputs.conf (the indexer version) on the indexer (Splunk server)
	- /opt/splunk/etc/system/local/inputs.conf
- Place props.conf on the indexer
	- /opt/splunk/etc/system/local/props.conf
- Place transforms.conf on the indexer
	- /opt/splunk/etc/system/local/transforms.conf

## If you are going to use an Intermediate Forwarder to relay logs, vs allowing all of your hosts to directly access Security Onion, [read the instructions here](https://github.com/bonifield/splunk_on_security_onion/tree/master/intermediateforwarder-files), and don't forget to change your endpoint outputs.conf IP addresses to the Intermediate Forwarder.

### On Endpoints
- Place [sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon) on the host, somewhere users don't have access to (NOT C:\Users)
- Place sysmon-config-sosalpha-JB-MODS.xml with sysmon, for sake of ease
	- This configuration is a derivative of the alpha config developed by [SwiftOnSecurity](https://github.com/SwiftOnSecurity/sysmon-config) and has been tuned for way more logging
- Install Sysmon
	- or better yet, use a GPO
```
sysmon64.exe -i -n -accepteula
sysmon64.exe -c sysmon-config-sosalpha-JB-MODS.xml
OR over the network
\\ServerName\sysmon -i -n -accepteula
sysmon -c sysmon-config-sosalpha-JB-MODS.xml
```
- Place the Universal Forwarder on the host and install it silently
	- Substitute INDEXER_IP_ADDRESS for your Splunk's network address
	- remove SERVICESTARTTYPE to make the forwarder automatically start after install, which is less optimal for scripts but convenient for single, step-by-step installs
```
msiexec.exe /i [yoursplunkforwarder].msi RECEIVING_INDEXER="INDEXER_IP_ADDRESS:9997" SERVICESTARTTYPE=manual AGREETOLICENSE=Yes /quiet
```
- Place inputs.conf (the endpoint version) on the host (don't forget that spaces in filepaths are lame...)
	- "C:/Program Files/SplunkUniversalForwarder/etc/system/local/inputs.conf"
- Place outputs.conf (the endpoint version) on the host
	- "C:/Program Files/SplunkUniversalForwarder/etc/system/local/outputs.conf"
		- substitute IPs as necessary for your indexer
- Configure the Forwarder to start automatically on boot, then start (or restart) the Forwarder
```
sc config SplunkForwarder start=auto
"C:\Program Files\SplunkUniversalForwarder\bin>splunk.exe" start
```
- Verify Splunk is both receiving logs from Windows endpoints, and indexing local Zeek (Bro) logs
```
Splunk homepage --> Searching and Reporting --> Data Summary --> Sourcetypes
```

### Randoms
- start/stop/restart Splunk
```
sudo /opt/splunk/bin/splunk start
sudo /opt/splunk/bin/splunk stop
sudo /opt/splunk/bin/splunk restart
```
- Clean old data in an index, and re-index all local files in any monitored folders
	- Typically if Zeek logs need to be purged because you forgot to convert output to JSON
```
sudo ./splunk stop
sudo ./splunk list index
sudo ./splunk clean eventdata -index _thefishbucket
sudo ./splunk clean eventdata -index [index]
sudo ./splunk start
```
- Force Splunk to recognize new changes to props.conf and/or transforms.conf
	- Try these things IN THIS ORDER
```
Search Bar --> type (yes with a leading pipe)	| extract reload=T
sudo /opt/splunk/bin/splunk restart
```

- Image = official term for a compiled binary file
- Subject_* = the one performing the action in question; the account requesting a logon (service, user, etc) but NOT the actual account logging on
- Target_* and User_* = the one being acted upon by the Subject
- New_Logon_* = the account for whom the new logon was created, i.e. the account logged on
