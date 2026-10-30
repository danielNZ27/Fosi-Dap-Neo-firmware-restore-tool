# Fosi DAP Neo Firmware Restore Tool

A low-level recovery and stock firmware restore utility for the **Fosi Audio DAP Neo** digital audio player. 

This tool communicates directly with the onboard Rockchip processor over the USB **Rockusb** recovery protocol to re-flash the factory firmware image (`DP3_V137.img`) to both system boot slots (Slot 1 at sector 0 and Slot 2 at sector 30720) in the event of a soft-brick, corrupted partition, or when reverting from experimental firmware.

---

## Prerequisites & Dependencies

1. **Operating System:**
   - Windows 10 or Windows 11 (the script utilizes Windows Win32 Overlapped I/O and device registry discovery).
2. **Python:**
   - Python 3.7 or newer.
   - **No external pip packages required** (uses standard library modules: `ctypes`, `winreg`, `struct`, `hashlib`, `pathlib`).
3. **Rockchip USB Driver:**
   - The DAP Neo must enumerate as a Rockusb device (`VID_2207&PID_262D`).
   - If Windows does not recognize the device in recovery mode, install the standard **Rockchip DriverAssistant** (`rockusb.sys`).

---

## Step-by-Step Restoration Guide

### 1. Boot the DAP Neo into Recovery Mode
1. **Unplug** the USB-C cable from the player.
2. If the player is powered on, press and hold the **Power** button for 3–5 seconds until the screen turns completely off.
3. Press and hold **VOL +**, **VOL -**, and the **Center / OK** button simultaneously (do **not** touch the Power button).
4. While continuing to hold those three buttons, **plug the USB-C cable into your PC**.
5. Keep holding the three buttons for **3 to 5 seconds** after connecting the cable, then release them.
6. The screen will remain **completely black** — this confirms the player has successfully entered hardware Rockusb recovery mode.

### 2. Run the Restore Script
1. Clone or download this repository to your computer.
2. Open a Command Prompt or PowerShell terminal inside the folder containing `restore-stock.py` and `DP3_V137.img`.
3. Run the restore script:
   ```cmd
   python restore-stock.py
   ```
4. The tool will:
   - Locate the Rockusb interface in Windows.
   - Handshake with the Rockchip processor and query chip/flash IDs.
   - Flash the factory firmware image to **Slot 1 (sector 0)**.
   - Flash the factory firmware image to **Slot 2 (sector 30720 / 15 MiB offset)**.
   - Verify the written sectors and issue a reset signal.

### 3. Reset and Normal Boot
1. When the script displays `[COMPLETE] Player restarted into stock firmware!`:
2. **Unplug** the USB cable from the DAP Neo.
3. Use a **paperclip** (or SIM ejector tool) to gently press the physical **Reset button** located next to the USB-C port.
4. Press the **Power** button to boot normally. The stock Fosi Audio operating system will load with internal storage and SD card functionality fully restored.

---

## Firmware Information

- **Included Firmware:** `DP3_V137.img` (Stock DP3 V1.37 factory firmware)
  - **SHA-256:** `fcc1cd3e35b46f1f8dbcb8da39fcc5eb3f1d469cdfade5e9c6ea3bd71ac6deef`
- **Official Releases:**
  - For the latest firmware versions, release notes, and official updates, please refer directly to [Fosi Audio's Official Website and Support](https://fosiaudio.com).

---

## Troubleshooting

- **Error: `Expected 1 Rockusb recovery interface, found 0`:**
  - The DAP Neo is either not in recovery mode or the USB driver is missing.
  - Check Windows Device Manager under *Class for rockusb devices* or *Universal Serial Bus controllers* to verify `Rockusb Device` is listed with Hardware ID `VID_2207&PID_262D`.
  - Repeat the 3-button plug-in sequence (VOL +, VOL -, OK) carefully.
- **Permission Errors:**
  - If you encounter access denied errors accessing USB pipes, run Command Prompt or PowerShell as **Administrator**.

---

## License & Disclaimer

This tool is provided for recovery and maintenance purposes. Flashing firmware always carries inherent risks; ensure all connections remain stable during the flashing process.
