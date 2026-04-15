# Magcubic HY300 Root

Rooting and firmware customization guide for Magcubic / HY300 Pro projectors, including Magisk integration, HDMI input state detection, IR remote override, and optional debloating.

## Overview

This repository documents a complete workflow for rooting Magcubic / HY300 Pro–type projectors, rebuilding the firmware, and installing Magisk modules to:

- detect HDMI input plug/unplug events
- automatically switch to HDMI input
- override IR remote keys such as volume and power

The goal is to keep a reproducible reference for rebuilding and restoring the setup later without trial and error.

## Requirements

- Magcubic / HY300 Pro projector (Allwinner / Sunxi platform)
- Windows or Linux PC with ADB
- USB debugging enabled
- Original firmware image provided by the manufacturer
- Magisk APK for post-flash installation

> Rooting may void the device warranty and can permanently brick the device if performed incorrectly.

## Enable Developer Options

1. Open **Settings → About device**
2. Tap **Build number** 7 times
3. The **Developer options** menu should appear

## Enable USB Debugging

```text
Settings → System → Developer options → USB debugging
````

Enable USB debugging and leave it turned on during the procedure.

## Firmware extraction and patching

### 1. Obtain the original firmware

Request the full firmware package from the manufacturer.
It is typically provided as an `.img` file.

### 2. Extract `boot.fex`

Use `imagewty-tool` to unpack the firmware image.

* On Windows, run it through WSL if needed
* Extract the `.img` file
* The tool should generate a folder ending in `.dmp`
* Inside the extracted dump, locate `boot.fex`

### 3. Download Magisk

Download the official Magisk APK and keep it for later installation on the projector.

### 4. Patch `boot.fex`

Patch the extracted `boot.fex` using a compatible Magisk patching workflow.

Recommended patching steps:

1. Select the extracted `boot.fex`
2. Enable all relevant options except `recovery`
3. Run the patching process
4. Download the patched output
5. Rename the patched file back to `boot.fex`
6. Replace the original `boot.fex` inside the extracted `.dmp` directory

### 5. Repack the rooted firmware

Rebuild the modified dump into a new rooted firmware image, for example:

```text
update_rooted.img
```

This rebuilt image will be flashed with PhoenixSuit.

## Windows setup: PhoenixSuit driver installation

1. Disable **Windows Defender Core Isolation** if unsigned USB drivers fail to install
2. Install the PhoenixSuit drivers
3. Open **Device Manager** and verify that the device is recognized correctly

If the driver is not installed properly:

* right-click the device
* choose **Update driver**
* select **Browse my computer for drivers**
* point to the PhoenixSuit driver directory

The device should appear without any warning icon.

## Enter FEL mode

Use the following sequence to put the projector into FEL mode:

1. Power off the projector
2. Unplug USB
3. Unplug power
4. Hold the **Power** button on the remote for about 5 seconds
5. While still holding it, plug the USB cable into the PC
6. Wait about 10 seconds
7. Plug in the power adapter
8. Wait a few seconds for the projector to power on
9. Release the power button

At this point, the PC should detect the projector in FEL mode.

## Flash the rooted image

1. Launch **PhoenixSuit**
2. Select the rebuilt image: `update_rooted.img`
3. Start the flashing process

Once flashing completes and the projector reboots, the patched boot image should be installed.

## Finish Magisk setup

1. Install or open the **Magisk APK** on the projector
2. Let Magisk complete its initial setup
3. Reboot if requested

Root access should now be fully operational.

## Install Magisk modules

Custom Magisk modules can be used to extend the projector behavior, for example:

* detect HDMI input state changes
* automatically switch to HDMI input
* intercept IR remote commands such as power and volume
* forward IR actions to an external device such as a Fire Stick
* block unwanted hosts or services

This allows a simplified setup where one remote can control both the projector and the connected HDMI device.

## Result

After completing the process, the projector is:

* rooted with a Magisk-patched `boot.fex`
* configured for Magisk module support
* able to detect HDMI input state changes
* ready for automatic HDMI switching
* capable of IR key override for custom remote behavior
