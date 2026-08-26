> First we need to stop monitor mode:

```
sudo airmon-ng stop wlan0mon
```

> Now we need to make our conf file for wpa supplicant: 

```
sudo cat > wep.conf << EOF
network={
    ssid="<ssid>"
    key_mgmt=NONE
    wep_key0=<key_no_colons>
    wep_tx_keyidx=0
}
EOF
```

```
cat wep.conf
```

> Once the file is good we can run wpa supplicant:

```
sudo wpa_supplicant -D nl80211 -i wlan0 -c wep.conf -B
```

> Now we can get a valid IP address on the wlan0 interface:

```
sudo dhclient wlan0 -v
```

> Once it says its bound to an IP address, we can retrieve the proof.txt file flag:

```
curl http://<ip>/proof.txt
```

