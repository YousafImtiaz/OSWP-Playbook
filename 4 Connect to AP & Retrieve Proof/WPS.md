> First we need to stop monitor mode:

```
sudo airmon-ng stop wlan0mon
```

> Now we need to make our conf file:

```
echo 'network={
    ssid="<ssid>"
    psk="<password>"
}' > wps.conf
```

```
cat wps.conf
```

> Once the file is good we can run wpa supplicant:

```
sudo wpa_supplicant -i wlan0 -c wps.conf -B
```

> Now we can get a valid IP address on the wlan0 interface:

```
sudo dhclient wlan0 -v
```

> Once it says its bound to an IP address, we can retrieve the proof.txt file flag:

```
curl http://<ip>/proof.txt
```
