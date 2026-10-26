
Gateware development
Prerequisites: 
- Ubuntu 24.04
- Vivado 2024.2 (path: ~/opt/Xilinx/Vivado/2024.2)

<br>
Building the board binaries

```bash
python3 -m artiq.gateware.targets.kasli [Path to crate config file (json)] 
```

<br>
Confirn openocd location
```bash
which openocd
# output would look something like this:
/nix/store/i0f3cm13z9g5q3ns3253kcvhich5yh0l-openocd-bscanspi/bin/openocd
```
<br>

Setup udev. 
- This is a one-time process. If you have flashed before from your system, you don't need to do this again.
```bash
sudo cp /nix/store/i0f3cm13z9g5q3ns3253kcvhich5yh0l-openocd-bscanspi/share/openocd/contrib/60-openocd.rules /etc/udev/rules.d 
 
sudo udevadm trigger
```
<br>

Flashing
- Connect your computer to kasli via micro-usb cable that can handle data transfer.<br>
**crate_name: testbed, castor, lri**
```bash
artiq_flash -d artiq_kasli/[crate_name] --srcbuild
```

Clocking over USB
```bash
artiq_mkfs storage.img -s ip [IP]/24 -s rtio_clock ext0_synth0_10to125
artiq_flash write=storage start -f storage.img
```

Creating new device_db
```bash
artiq_ddb_template [master json] -s 1 [satellite json] -o
```
