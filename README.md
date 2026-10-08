# Firmware_help



From your kernel log:

```
idVendor=0cf3, idProduct=9271
Product: UB91C
Manufacturer: ATHEROS

ath9k_htc: Firmware htc_9271.fw requested
Direct firmware load for htc_9271.fw failed with error -2

no suitable firmware found!
```

The driver (`ath9k_htc`) is already present in the kernel.

**The problem is that the firmware file is missing.**

## Check your Linux distribution

First run:

```bash
cat /etc/os-release
```

If you are using **Debian**, install the firmware package:

```bash
sudo apt update
sudo apt install firmware-atheros
```

If you are using a newer Debian release where firmware is separated:

```bash
sudo apt install firmware-atheros
```

Then unplug and reconnect the Alfa adapter, or reload the module:

```bash
sudo modprobe -r ath9k_htc
sudo modprobe ath9k_htc
```

Or simply reboot:

```bash
sudo reboot
```

## Verify that the firmware loaded

Run:

```bash
dmesg | grep ath9k
```

You should see something similar to:

```
ath9k_htc: Firmware htc_9271.fw requested
ath9k_htc: HTC initialized
```

and **not**:

```
failed to load htc_9271.fw
no suitable firmware found
```

## Check whether the interface appears

```bash
ip link
```

or

```bash
iw dev
```

You should see a wireless interface such as:

```
wlan0
```

or

```
wlx...
```

---

If `firmware-atheros` is already installed but the firmware is still missing, paste the outputs of:

```bash
cat /etc/os-release
dpkg -l | grep firmware-atheros
ls -l /lib/firmware/ath9k_htc/
```

and I can help diagnose the next step.
