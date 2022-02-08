# Using ipset to implement firewall rules to a group of IP Addresses ( really useful if you hate using CIDR)

You have to install ipset first which you can do simply by
```
opkg update
opkg install ipset
```

Rules you have to add in /etc/config/firewall to first assign a group of ip addresses to a ``option name``, which you can later use to apply certain rules to them
```
config ipset
        option name 'test'
        option match 'src_net'
        option storage 'hash'
        option enabled '1'
        option loadfile '/mnt/ips/test.txt'

config rule
        option name 'test'
        list proto 'all'
        option src 'lan'
        option ipset 'test'
        option dest 'wan'
        option target 'REJECT'
        option enabled '0'

``` 
# Interacting or modifying the existing rules on /etc/config/firewall directly from shell with commands

```
uci set firewall.@rule[13].enabled=1 && uci commit && /etc/init.d/firewall restart &>/dev/null && echo

```
firewall.@rule[] points to the rule you want to change. You can list the rule ids that is 13 there, by using the command ``uci show firewall``. Keep in mind that every rules you put in the /etc/config/firewall has different numerical id in a ascending order. After that, there is ``.enabled=1``. This is the value we are changing and you have to put it based on what you want. 

With this you can group a large number of users and assign them a certain range of IPs with MAC Addreses and control their interet usage. With a simple bash script and cronjob, you can have your own small time based ISP. 
