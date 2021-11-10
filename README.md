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

## usage

Simple data usage logger script

### installation with cron

monitor every minute:

    - * * * * /mnt/usb/monitor_usage.sh eth0

### usage log format

The log is written to a file called `usage_[DEVICE].log`:

    DA TI TX_C RX_C SUM TX_L RX_L SUM_L TX_M RX_M SUM_M 

`DA`     current date
`TI`     current time
`TX_C`   tx_bytes as currently reported by the kernel (eventually adjusted due to reboots)
`RX_C`   rx_bytes as currently reported by the kernel (eventually adjusted due to reboots)
`SUM`    sum of TX_C and RX_C
`TX_L`   tx_bytes since last measured (increment)
`RX_L`   rx_bytes since last measured (increment)
`SUM_L`  sum of TX_L and RX_L
`TX_M`   tx_bytes since beginning of this month (actually this log file)
`RX_M`   rx_bytes since beginning of this month
`SUM_M`  sum of TX_M and RX_M

**example log line:**

2021-10-18 11:38:00 31832148 1191376080 1223208228 6528 266613 273141 76961 2174380 2251341

### current usage (`/run`)

In addition to the usage file, the current usage for the month is written to the files `/run/usage_[DEVICE].rx` (recieved), `/run/usage_[DEVICE].tx` (sent) and `/run/usage_[DEVICE]` (sum).  