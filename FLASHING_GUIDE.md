# 📘 **COMPLETE GUIDE TO FLASHING GSI VIA ODIN (Windows) AND HEIMDALL (Linux/Mac)**

This guide is suitable for all Samsung devices with an unlocked bootloader and Project Treble (GSI) support.

---

# ⚠️ **Disclaimer**

**You flash at your own risk. The author of this guide is not responsible for any device damage, data loss, or malfunction.**

---

# ✅ **1. Requirements**

- Samsung device with unlocked bootloader  
- PC with Windows or Linux/Mac  
- Odin (for Windows)  
- Heimdall (for Linux/Mac)  
- Samsung USB drivers  
- USB cable  
- **Archive containing super.img** (and boot.img if available) — downloaded GSI  
- Recommended: fully charged phone  

> ⚠️ When downloading the GSI, you will receive an **archive**.  
> You **do NOT need to fully extract it** — extraction is required only once.  
>
> • On **Windows (Odin)**: flash the `LineageOs20/21.tar` file as it is.  
> • On **Linux / macOS (Heimdall)**: extract `LineageOs20/21.tar`, and inside you will get  
>   `boot.img` (if included) and `super.img` — these are the files you must flash.

---

# 🧰 **2. Device Preparation**

## **2.1. Enable Developer Options**

1. Settings → About phone  
2. Tap **Build number** 7 times  
3. In developer options enable:
   - **OEM Unlocking**
   - **USB Debugging**

---

## **2.2. Unlock Bootloader**

Make sure the phone is charged to at least 80%.
Do NOT connect the USB cable yet! If it is connected, disconnect it, otherwise the phone will go into charging mode.

1. Power off your phone completely and wait 10 seconds.
2. Press and hold **Volume Down + Power**.
3. When the Samsung logo appears, release the **Power** button but keep holding **Volume Down** until the blue warning screen appears.
4. **Connect the USB cable** to your PC.
5. **LONG PRESS Volume Up** to enter "Device unlock mode".
6. Press **Volume Up** once to confirm. 
7. The device will wipe data, unlock the bootloader, and reboot automatically.

---

## **2.3. Wipe Data**

*(Wait for the device to fully boot after unlocking, then power it off again).*
1. Enter Recovery (**Volume Up + Power**)  
2. Select **Wipe data / factory reset**
3. Select **Factory data reset** to confirm

---

## **2.4. Enter Download Mode**

1. Power off the device  
2. Press and hold **Volume Down + Volume Up** and **connect the USB cable**.
3. When the blue warning screen appears, release the buttons.
4. **SHORT PRESS Volume Up** to enter Download Mode for flashing.

---

# 🪟 **3. Flashing via Odin (Windows)**

## **3.1. Download and Install**

- [Download Odin (recommended version 3.14.4)](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/%D0%9Edin)  
- [Install Samsung USB Drivers](https://developer.samsung.com/android-usb-driver)  

---

## **3.2. Odin Setup**

1. Launch Odin  
2. Connect your device → "Added!!" should appear  
3. Click **AP** → select the **.tar archive**  
4. Keep only these options checked:
   - **Auto Reboot**
   - **F. Reset Time**

Do **NOT** enable:
- Re-Partition  
- Nand Erase  

---

## **3.3. Start Flashing**

Click **Start** in Odin and wait 1–3 minutes.  
If successful, you will see **PASS!**

---

## **3.4. Verify Result**

The device will reboot automatically.  
If it boots → GSI is installed.  
If bootloop occurs → perform Wipe Data in Recovery.

---

# 🐧 **4. Flashing via Heimdall (Linux/Mac)**

## **4.1. Install Heimdall**

### Ubuntu/Debian:

```bash
sudo apt install heimdall-flash
```

### Arch Linux:

```bash
sudo pacman -S heimdall
```

### macOS (brew):

```bash
brew install heimdall
```

---

## **4.2. Prepare Files for Flashing**

1. You downloaded a `.tar` archive with the GSI.  
2. Extract it:

```bash
tar -xf archive_name.tar
```

3. Inside you will get one or two files:  
   - **boot.img** (if available)  
   - **super.img**

4. Go to the folder containing these files:

```bash
cd ~/path_to_extracted_files
```

---

## **4.3. Check Connection**

Device must be in **Download Mode**:

```bash
heimdall detect
```

If Heimdall shows *Device detected* — ready to flash.

---

## **4.4. Flash super.img**

```bash
sudo heimdall flash --super super.img
```

> On some Samsung devices, the partition may be named `--super` or `--SUPER`.  
> If it fails, try both options.  
> If needed, you can also flash **boot.img**:

```bash
sudo heimdall flash --boot boot.img
```

---

## **4.5. Completion**

Heimdall will show progress and automatically reboot the device.  

After reboot, ensure the device boots and the GSI is installed.

---

## **4.6. Verify Installation**

- Device boots → everything is OK  
- Bootloop → perform Wipe Data  
- Flashing error → check GSI compatibility  

---

# 🎉 Done!

**The GSI is now installed and ready to use.**
