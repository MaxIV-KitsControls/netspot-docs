# API

It's possible to query the database using a REST API. The API is web browsable. These are the avaiable calls at the moment.

## IP -> MAC lookup

You can find out the MAC address of an IP address by the following call:

Example: To find the MAC address for 172.17.101.10:

    http://netspot/api/v1/getmac/172.17.101.10/

### Python example

```python
#!/usr/bin/python -tt
 
import json
from urllib2 import urlopen
 
url = "http://netspot/api/v1/getmac/172.17.102.10/"
response = urlopen(url)
 
raw_data = response.read().decode('utf-8')
 
for mac in json.loads(raw_data):
  print mac
```

## NetMagis Search

It's possible to search NetMagis using the NetSPOT API.

Example to find IP and MAC for hostname "w-v-netconf-0":

  https://netspot.maxiv.lu.se/api/v1/netmagis_search/w-v-netconf-0

### Python example

```python
#!/usr/bin/python -tt

import json
from urllib2 import urlopen

search = 'w-v-netconf-0'

url = 'https://netspot.maxiv.lu.se/api/v1/netmagis_search/{0}/'.format(search)
response = urlopen(url)

raw_data = response.read().decode('utf-8')

for record in json.loads(raw_data):
  print record
```

Output:
```
$ ./test.py
{u'mac': u'00:50:56:b3:07:77', u'hostname': u'w-v-netconf-0', u'ip_address': u'192.168.19.59'}
```
