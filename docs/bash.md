# BASH

## AWK

### Get the line from row number
```
awk 'NR==<linenumber>' <filename>
```

## CD

### Return to the previous working dir.
```
cd -
```

## Tree
### Show only directories
```
tree -d
```

### Limit the depth of directory
```
tree -L <linenumber>
```

## Shell History
### Forgot to run the command as root?
You can rerun the last command as root by writing
```
sudo !!
```

### Run the last command that starts with
```
!<string>
```

If your last command was uptime you can write
```
!u
```
And it will automatically find last command that starts with u and run it.

### Reuse the last commands first parameter
```
!^
```
e.g
```
host google.com

google.com has address 216.58.201.174
google.com has IPv6 address 2a00:1450:400f:805::200e
google.com mail is handled by 50 alt4.aspmx.l.google.com.
google.com mail is handled by 40 alt3.aspmx.l.google.com.
google.com mail is handled by 10 aspmx.l.google.com.
google.com mail is handled by 20 alt1.aspmx.l.google.com.
google.com mail is handled by 30 alt2.aspmx.l.google.com.
```

```
ping !^

ping google.com
PING google.com (216.58.201.174) 56(84) bytes of data.
64 bytes from arn02s06-in-f174.1e100.net (216.58.201.174): icmp_seq=1 ttl=56 time=18.9 ms
64 bytes from arn02s06-in-f174.1e100.net (216.58.201.174): icmp_seq=2 ttl=56 time=19.8 ms
64 bytes from arn02s06-in-f174.1e100.net (216.58.201.174): icmp_seq=3 ttl=56 time=22.4 ms
64 bytes from arn02s06-in-f174.1e100.net (216.58.201.174): icmp_seq=4 ttl=56 time=22.9 ms
^C
```

### Reuse the last commands last parameter
```
!$
```