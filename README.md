# Sonar

## How to use code?
1) Install python 2.
- Python 2: https://www.python.org/downloads/
- Note: Code will also execute on python3, however some of the code may need to be changed to python3 syntax.
2) Install smbus2 with pip:
```
pip install smbus2
```
3) See technical documentation for i2c connection between the sonar and raspberry pi.

### Code:

```python
from smbus2 import SMBus #I2C
from datetime import datetime #Date
import time #Delay

i2cAddr = 0x70 #Sensor i2c address

writeRangeCmd = 0x51 #Write range Command. 81 in decimal
initRead = 0xe1 #Initiate read. 225 in decimal

delay1 = 0.08 #80ms
delay2 = 0.10 #100ms

while True:
    try:
        i2cbus = SMBus(1)
        i2cbus.write_byte_data(i2cAddr, 0, writeRangeCmd) #Write the range command byte.
        time.sleep(delay1)
        rawData = i2cbus.read_word_data(i2cAddr, initRead) #Initiate a read at the sensor address. Word = 2bytes.
        rangeValue = (rawData >> 8) & 0xff #Right shift 8-bits. Mask with 0x00ff.
        print '---------------------------------'
        print rangeValue, 'cm'
        print datetime.now()
        print '---------------------------------'
    except IOError as err:
        print(err)
    time.sleep(delay2)
```
4) Execute code in the command window:

- i2c permission:
```
sudo chmod a+rw /dev/i2c-*
```
- Run script:
```
python2 sonar_example.py
```
