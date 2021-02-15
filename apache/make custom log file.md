# Create Custom Log location in Apache

## Look at log types in apache.conf
1. open `/etc/apache2/apache.conf`
2. There will be a section with `LogFormat` that defines all of the named types of logs. 
	- for example, if I had a file like:
```
---
LogFormat "%v:%p %h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" vhost_combined
LogFormat "%h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" combined
LogFormat "%h %l %u %t \"%r\" %>s %O" common
LogFormat "%{Referer}i -> %U" referer
LogFormat "%{User-agent}i" agent
---
```

	Then I have logFormats defined for vhost_combined, combined, common, referer, and agent

3. Exit apache.conf file

## Modify Your sites file

1. open `/etc/apache2/sites-enabled/---`
2. 
