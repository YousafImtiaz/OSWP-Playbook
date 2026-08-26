> View interface:

```
iwconfig
```

> Now we can kill any processes so they dont interfere and then start monitor mode:

```
sudo airmon-ng check kill & sudo airmon-ng start wlan0
```

> Confirm monitor mode and check the created interface using:

```
iwconfig wlan0mon
```
