# istation-scripts
Useful scripts for Kerlink Wirnet iStation LoRaWAN Gateway.

## humidity

Reports humidity from the onboard sensor.

Example:

```shell
$ humidity
8.90502
```

## ipaddr

Reports the IP v4 address of the given device (default `eth0`).

Example:

```shell
$ ipaddr vpn0
192.168.1.3
```

## smsip

Monitors IPs and sends an SMS if a change is detected.

Example:

```shell
$ smsip 004917112345678
```

The IPs are stored at `/var/run/` in files like `vpn0.ip`. If these files are missing or if one of the IPs to monitor has changed, the internal GSM modem is used to send out an SMS in the form:

```
[hostname] ip change detected
wwan0: [IP]
eth0: [IP]
vpn0: [IP]
```

To regularly check for IP changes, use `cron`:

```cron
* * * * * /usr/bin/smsip 004917112345678
```

## temperature

Reports internal (CPU, default) and external (enclosure) temperatures in degree celsius.

Example:

```shell
# cpu is default
$ temperature
72.981
# specify cpu sensor
$ temperature cpu
72.981
# enclosure temperature
$ temperature enclosure
55.5417
```