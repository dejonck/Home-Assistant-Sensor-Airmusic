# Home-Assistant-Sensor-Airmusic
AirMusic Envivo radio ENV-1388

**Note:**
Not all models supporting Airmusic are having the same ability of curl commands. As example on this version the Sendkeys is not available.
The authorization key, `'Authorization: Basic c3UzZzRnbzZzazc6amkzOTQ1NHh1L14='` as described for many, is as well not required on this model.

**resources:**
* https://www.teknihall.be/sites/teknihallbe/files/downloads/1388.pdf
* https://community.home-assistant.io/t/airmusic-control-by-mediau/388429
* https://github.com/RobinMeis/AirMusic

**Curl commands and their XML output**

* Get system info
```
$ curl http://192.168.0.97/GetSystemInfo
```
`
<?xml version="1.0" encoding="UTF-8"?><menu><SW_Ver>SW809H11-f828n-f903**a*-f811a-5b11</SW_Ver><wifi_info><status>connected</status><MAC>20F41B15F0CB</MAC><SSID>spynet</SSID><IP>192.168.0.97</IP><Subnet>255.255.255.0</Subnet><Gateway>192.168.0.1</Gateway ><DNS1>8.8.8.8</DNS1><DNS2></DNS2></wifi_info></menu>
`

* Get the list of the main menu (id=1)
```
$ curl "http://192.168.0.97/list?id=1&start=1&count=250"
<?xml version="1.0" encoding="UTF-8"?><menu><item_total>5</item_total><item_return>5</item_return><item><id>87</id><status>content</status><name>Lokale radio</name></item><item><id>52</id><status>content</status><name>Internet-radio</name></item><item><id>2</id><status>content</status><name>Mediacentrum</name></item><item><id>5</id><status>content</status><name>FM</name></item><item><id>47</id><status>content</status><name>AUX</name></item></menu>
```

* Get the list of Favorites (id=75)
```
$ curl "http://192.168.0.97/list?id=75&start=1&count=250"
<?xml version="1.0" encoding="UTF-8"?><menu><item_total>6</item_total><item_return>6</item_return><item><id>75_0</id><status>file</status><name>Q-music</name></item><item><id>75_1</id><status>file</status><name>JOE fm</name></item><item><id>75_2</id><status>file</status><name>Radio VRD 106.5 FM</name></item><item><id>75_3</id><status>file</status><name>Joe FM 80s</name></item><item><id>75_4</id><status>emptyfile</status><name>Leeg</name></item><item><id>75_5</id><status>emptyfile</status><name>Leeg</name></item></menu>
```

* Get the list of HotKeys (id=75)
```
$ curl "http://192.168.0.97/hotkeylist"
<?xml version="1.0" encoding="UTF-8"?><menu><item_total>6</item_total><item_return>5</item_return><item><id>75_0</id><status>file</status><name>Q-music</name></item><item><id>75_1</id><status>file</status><name>JOE fm</name></item><item><id>75_2</id><status>file</status><name>Radio VRD 106.5 FM</name></item><item><id>75_3</id><status>file</status><name>Joe FM 80s</name></item><item><id>75_4</id><status>emptyfile</status><name>Leeg</name></item></menu>
```

* Select the item from the main Menu
```
$ curl "http://192.168.0.97/gochild?id=52"
<?xml version="1.0" encoding="UTF-8"?><result><id>52</id></result>
```

* Play hotkey 1
```
$ curl "http://192.168.0.97/playhotkey?key=1"           
<?xml version="1.0" encoding="UTF-8"?><result><id>75</id><rt>OK</rt></result>
```

* Status of the radio
```
$ curl http://192.168.0.97/playinfo"
<?xml version="1.0" encoding="UTF-8"?><result>FAIL</result>
```

* Volume on 5 ond Mute turned off (vol 0->15)
```
$ curl "http://192.168.0.97/setvol?vol=5&mute=0"
<?xml version="1.0" encoding="UTF-8"?><result><vol>5</vol><mute>0</mute></result>
```

* Adding a new station
```
$ curl "http://192.168.0.97/AddRadioStation?name=BusinessAM&url=https://stream.medianation.be/bam"
<?xml version="1.0" encoding="UTF-8"?><result><rt>NO_SUPPORT</rt></result>
```

* Going back in the menu
```
$ curl "http://192.168.0.97/back"                        
<?xml version="1.0" encoding="UTF-8"?><result><id>75</id></result>
```

* Info of the current status
```
$ curl "http://192.168.0.97/playinfo"                    
<?xml version="1.0" encoding="UTF-8"?><result><vol>6</vol><mute>0</mute><status>Bezig met Afspelen</status><sid>6</sid<logo_img>http://192.168.0.97:8080/playlogo.jpg</logo_img><stream_format>MP3 /128 Kbps</stream_format><station_info> Joe/ Joe</station_info><song>Don't Stop</song><artist>FLEETWOOD MAC</artist></result>
```
