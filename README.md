# pdnd-picoprobe

A fork of Picoprobe, an SWD progammer firmware, for the Pico Debug'n'Dump.

## Usage

- Make sure the "Mode" switch is set to I2C/SWD
- Follow general OpenOCD instructions such as this: https://visualgdb.com/tutorials/raspberry/pico/picoprobe/

## Building

```
mkdir build
cd build
cmake ..
make
```

And just copy over the pdnd_picoprobe.uf2 file!


