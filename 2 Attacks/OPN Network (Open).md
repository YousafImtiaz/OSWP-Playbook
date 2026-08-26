> If we have monitor mode running and we find OPN then we can note the ESSID and then stop monitor mode:

```
sudo airmon-ng stop wlan0mon
```

> Now we can connect to it since its open:

```
echo 'network={
  ssid="<essid>"
  key_mgmt=NONE
}' > opn.conf
```

> Now we can run wpa_supplicant: 

```
sudo wpa_supplicant -i wlan0 -c opn.conf -B
```

> Now we can get a valid IP address:

```
sudo dhclient wlan0 -v
```

> Retrieve proof file:

```
curl http://<ip>/proof.txt
```
