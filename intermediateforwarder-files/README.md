# Files that get installed on an INTERMEDIATE FORWARDER which will receive endpoint logs, then relay them to the Indexer

## Instructions

### AFTER installing the Splunk Indexer, but BEFORE configuring your endpoints:

- note:  IntFwd = abbreviation for Intermediate Forwarder

- Install the appropriate Splunk Universal Forwarder on your preferred platform (this guide will reference Ubuntu)
```
sudo dpkg -i [splunkforwarder].deb
```
- Place inputs.conf (the IntFwd version) on the IntFwd server
	- /opt/splunk/etc/system/local/inputs.conf
- Place outputs.conf (the IntFwd version) on the IntFwd server
	- /opt/splunk/etc/system/local/outputs.conf
		- substitute IPs as necessary for your indexer
- Configure UFW (firewall) to allow forwarded logs to the Intermediate Forwarder
```
sudo ufw allow 9997/tcp
OR
sudo ufw allow proto tcp from [host-ip] to [indexer-ip] port 9997
```
- Start Splunk
```
sudo /opt/splunk/bin/splunk start
```
- Proceeed to install the endpoint configurations

### ON ENDPOINTS

- Follow the main instructions, but modify the endpoint's outputs.conf file to reflect the IntFwd IPs

### IF YOU HAVE MULTIPLE INTERFACES ON THE INTERMEDIATE FORWARDER
```
sudo ip route add [NET_IP_ADDRESS]/[CIDR] dev [YOUR_NET_FACING_INTERFACE]
sudo ip route add [INDEXER_IP_ADDRESS]/32 dev [YOUR_INDEXER_FACING_INTERFACE]
```

### Randoms
```
sudo ip route add i.i.i.i/cc dev INT
sudo ip route add i.i.i.i/cc via gw.gw.gw.gw dev INT
sudo ip route delete i.i.i.i/cc dev INT
```
