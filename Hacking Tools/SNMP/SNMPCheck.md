**Installation**
```
git clone https://gitlab.com/kalilinux/packages/snmpcheck.git
cd snmpcheck/
gem install snmp
chmod +x snmpcheck.rb
```

**Run the Tool**
```
/opt/snmpcheck/snmpcheck.rb VICTIM_IP -c COMMUNITY_STRING | more
```