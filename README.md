# splunk_on_security_onion

- stop Splunk and place files where necessary
```
sudo /opt/splunk/bin/splunk stop
...TODO...
sudo /opt/splunk/bin/splunk start
```

- TODO - make index via indexes.conf OR via GUI

- clean old data and re-index all files in any monitored folders
```
splunk list index
splunk clean eventdata -index _thefishbucket
splunk clean eventdata -index [index]
```

- view bro index on homepage
```
Settings --> Access Controls --> Roles --> [role] --> Indexes tab --> select the checkboxes in both Included and Default for desired index
```
