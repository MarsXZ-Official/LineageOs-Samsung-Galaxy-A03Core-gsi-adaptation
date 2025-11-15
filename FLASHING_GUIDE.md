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

> ⚠️ When downloading the GSI, you will get an **archive**. You **do not need to extract everything**, flash **only the super.img** (and boot.img if present) inside the archive.

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

1. Power off your device  
2. Press **Volume Down + Volume Up + connect USB**  
3. Press **Volume Up** to confirm  
4. The device will wipe data and unlock the bootloader

---

## **2.3. Wipe Data**

1. Enter Recovery (**Volume Up + Power**)  
2. Select **Wipe data / factory reset**

---

## **2.4. Enter Download Mode**

1. Power off the device  
2. Press **Volume Down + Volume Up + USB**  
3. Press **Volume Up** to enter

---

# 🪟 **3. Flashing via Odin (Windows)**

## **3.1. Download and Install**

- Download Odin (recommended version 3.14.4)  
- Install Samsung USB Drivers  

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
