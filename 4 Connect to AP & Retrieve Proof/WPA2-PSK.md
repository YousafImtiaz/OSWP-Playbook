> First we need to stop monitor mode:

```
sudo airmon-ng stop wlan0mon
```

> Now we can make our config file:

```
echo 'network={
  ssid="<essid>"
  psk="<password>"
  key_mgmt=WPA-PSK
}' > wpa2.conf
```

```
cat wpa2.conf
```

> Once the file is good we can run wpa supplicant:

```
sudo wpa_supplicant -i wlan0 -c wpa2.conf -B
```

> Now we can get a valid IP address on the wlan0 interface:

```
sudo dhclient wlan0 -v
```

> Once it says its bound to an IP address, we can retrieve the proof.txt file flag:

```
curl http://<ip>/proof.txt
```
