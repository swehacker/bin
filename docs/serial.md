## Serial communication

### Mac
Find out the "device" name of the USB device. Executing the command
```
ls /dev/tty.\*
```

#### Use screen as serial terminal
```
screen  /dev/tty.usbmodemfa2321 115200
```

#### For fully specify a USB port connection you can use:
```
screen  /dev/tty.usbmodemfa2321 115200,cs8,-cstopb,-parity,-crtscts
```

#### USB to serial
```
screen /dev/tty.KeySerial1 9600
```

#### Minicom
```
minicom -b 9600 -o -D /dev/ttyUSB0
```
