# Splunk on Security Onion

### Objectives:
- Manage logs with Splunk, versus the ELK stack built into Security Onion
- Ingest Windows Application, Security, System, and Sysmon logs via forwarding from remote hosts into Splunk on Security Onion
- Centrally monitor home or small office networks, especially for the purposes of home lab penetration testing and post-analysis
- Provide all necessary configuration files, and all necessary commands in a streamlined set of instructions

### What This IS:
- A manual, but streamlined, method of installing and configuring Splunk on a standalone instance of Security Onion
- A series of configuration files, and instructions on where to put them
- A fully functional project (note points below as to what is not yet included)
- A pet project
- A series of instructions that work across Debian-based Linux builds (Debian, Ubuntu, Security Onion, etc)

### What This IS NOT:
- A perfect solution
- An application (sure, apps are easy, but they don't show me how or why they work)
- A finished project (Snort and OSSEC parsers aren't included, nor even finished, as of this writing)
- A Splunk license (use the free one for your home lab)
- A series of instructions for RHEL/CentOS/Fedora/etc.  This is for Security Onion specifically, but it works just as well (identically) on Ubuntu.

### If using Ubuntu and not Security Onion, this requires that Zeek (Bro) produce logs in JSON format, and the monitor paths be changed as necessary in the server's inputs.conf file.  Otherwise, all configurations should work exactly the same.

### TODO
- Create scripts and/or GPOs to install the forwarders across a network
- Actually utilize Splunk's ability to create new (non-default) certificates when deploying to hosts
- Add hardening guide (unlikely)
- Repo for Win/Sysmon on ELK, the exact reverse of this project
- Field modifications:  CEF compliance, better extractions (index-time and search-time), MORE extractions/fixes as needed (there are definitely some I missed)

## INSTRUCTIONS






