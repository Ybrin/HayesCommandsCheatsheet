# Hayes Commands

All examples are made with a Raspberry Pi (Raspbian) and a Huawei E3372 LTE stick in modem mode, connected with a ppp connection on `/dev/ttyUSB0`. `/dev/ttyUSB2` is used for the AT commands.

The commands should behave similar on other devices/modems.

> See [this link](https://www.thefanclub.co.za/how-to/how-setup-usb-3g-modem-raspberry-pi-using-usbmodeswitch-and-wvdial) for more info

## Get signal strength

```bash
sudo chat -V -s '' 'AT+CSQ' 'OK' '' > /dev/ttyUSB2 < /dev/ttyUSB2
```

Returns something like the following:

```
+CSQ: 18,99
```

Take the number, remove the decimals and use the following table to convert to dBm

| number | dbM        |
| ------ | ---------- |
| 0      | < -113 dBm |
| 1      | -111 dBm   |
| 2      | -109 dBm   |
| 3      | -107 dBm   |
| 4      | -105 dBm   |
| 5      | -103 dBm   |
| 6      | -101 dBm   |
| 7      | -99 dBm    |
| 8      | -97 dBm    |
| 9      | -95 dBm    |
| 10     | -93 dBm    |
| 11     | -91 dBm    |
| 12     | -89 dBm    |
| 13     | -87 dBm    |
| 14     | -85 dBm    |
| 15     | -83 dBm    |
| 16     | -81 dBm    |
| 17     | -79 dBm    |
| 18     | -77 dBm    |
| 19     | -75 dBm    |
| 20     | -73 dBm    |
| 21     | -71 dBm    |
| 22     | -69 dBm    |
| 23     | -67 dBm    |
| 24     | -65 dBm    |
| 25     | -63 dBm    |
| 26     | -61 dBm    |
| 27     | -59 dBm    |
| 28     | -57 dBm    |
| 29     | -55 dBm    |
| 30     | -53 dBm    |
| 31     | > -51 dBm  |

> See [this link](https://mybroadband.co.za/forum/threads/how-to-check-signal-strength.61703/) for more info

## Get cell tower information

```bash
sudo chat -V -s '' 'AT+CREG?' 'OK' ''> /dev/ttyUSB2 < /dev/ttyUSB2
```

Returns something like the following:

```
+CREG: 2,1,"27E4","018FA464"
```

where the 3rd pice (the first string) is the LAC in HEX format and the 4th piece (the second string) is the CID in HEX format.

```bash
sudo chat -V -s '' 'AT+COPS?' 'OK' ''> /dev/ttyUSB2 < /dev/ttyUSB2
```

Returns something like the following:

```
+COPS: 0,2,"23205",7
```

where the 3rd piece (the first string) is the 3 digit MCC concatenated with the 2 digit MNC.

> See [this link](https://cellidfinder.com/mcc-mnc) for more info

To find a cell towers geo location from those values, use the following website: [Cellfinder](https://cellidfinder.com/)

## Get the connection technology

If you want to know whether you have an LTE, GSM,... you can do the following:

```bash
sudo chat -V -s '' 'AT+COPS?' 'OK' ''> /dev/ttyUSB2 < /dev/ttyUSB2
```

Returns something like the following:

```
+COPS: 0,2,"23205",7
```

where the last piece describes the technology (0=GSM, 1=WDMA, 7=LTE)

> See [this answer](https://www.lteforum.at/mobilfunk/at-command-execution-fuer-huawei-sticks.2235/post-34480) for more info
