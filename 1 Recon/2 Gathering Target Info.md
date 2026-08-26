> Start gathering traffic data:

```
sudo airodump-ng wlan0mon -w discovery --wps --band abg
```

> Here you want to note the following:

- BSSID
- ESSID
- Network Type (WPA2, MGT, WEP)
- Channel
- Station

Once we have this information we will start precision sniffing on our target:

```
sudo airodump-ng -c <channel> --bssid <bssid> --essid '<essid>' -w <outfile> wlan0mon
```
