# 📘 GSI Adaptation Guide for Samsung Galaxy A03 Core

> ℹ️ This guide can work **not only on Samsung Galaxy A03 Core**,  
> but also on **other Samsung / Samsung Galaxy devices**,  
> if they have the same partition structure (`boot.img`, `vendor.img`, `super.img`)  
> and firmware in AP format.

This guide is intended for **GSI adaptation** (LineageOS / AOSP) if flashing a lower version GSI on your device causes a **bootloop**.

---

## ⚠️ Important

- **You proceed at your own risk.**  
- The author is not responsible for bricks, data loss, or device damage.  
- This guide is intended **only for adaptation** if the author’s GSI version is lower than your device firmware.

> Adaptation goal: update `boot.img` (kernel) and `super.img` (system) so that GSI works correctly.

---

## 🔹 Step 1: Check your device firmware version

1. Go to **Settings → About phone / About device**.  
2. Find **Baseband / Modem version**.  
3. This version is critical for selecting the correct adaptation files.

> You need two files: `boot.img` (kernel) and `super.img` (system).

---

## 🔹 Step 2: Download required files

### 2.1 Device firmware

- **For A032F**: [SM-A032F firmware](https://samfw.com/firmware/SM-A032F)  
- **For A032M**: [SM-A032M firmware](https://samfw.com/firmware/SM-A032M)  

Download the **AP part** (file starting with `AP_...`) — it contains `boot.img` and `super.img`.

> ⚠️ You will need **two AP versions** for adaptation:
> 1. **AuthorVersion** — the AP version the GSI author used (or the one that caused the bootloop).  
> 2. **YourVersion** — the AP version installed on your device (matches your Baseband / Modem firmware).

This is important because the **GSI author may not include boot.img**, in which case you must extract `boot.img` from the corresponding AP version.

### 2.2 Tools

- **MagiskBoot**: [download](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/magiskboot)  
- **lpunpack / lpmake** for Linux/Ubuntu (to build super.img): [download](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/lpunpack_and_lpmake/bin)  
- On Windows, use `magiskboot.exe`  

---

## 🔹 Step 3: Prepare folder structure

Create a main folder for adaptation with two subfolders:

Adaptation/

├─ AuthorVersion/   # GSI version that caused bootloop

└─ YourVersion/     # your device firmware

- Copy `boot.img` **from the AuthorVersion AP** (or from the GSI archive if provided) into `AuthorVersion/`.  
- Copy your `boot.img` (from your AP firmware) into `YourVersion/`.  
- Copy `magiskboot.exe` into both folders.  

---

## 🔹 Step 4: Adapt boot.img on Windows

1. Open **CMD** in each folder.  
2. Unpack `boot.img`:

```bash
magiskboot unpack boot.img
````

* This extracts **dtb and other components**.

3. Copy DTB from your version to the author folder and vice versa (if needed).
4. Repack `boot.img`:

```bash
magiskboot repack boot.img
```

* You get `new-boot.img` — the adapted kernel.
* Keep `new-boot.img` in the same folder where `super.img` will be for flashing.

---

## 🔹 Step 5: Adapt super.img (Linux/Ubuntu)

**Recommendation:** use a Linux virtual machine (Ubuntu) for convenience and safety.

### 5.1: Install VirtualBox and Ubuntu

1. Download and install [VirtualBox](https://www.virtualbox.org/).
2. Install Ubuntu in VirtualBox (latest LTS version, e.g., 24.04 LTS).

### 5.2: Set up shared folders

To allow the VM to access files on your host:

1. Install **Guest Additions** in Ubuntu:

```bash
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)
# In VirtualBox: Devices → Insert Guest Additions CD → ISO mounts
sudo sh /media/<cdrom_mount>/VBoxLinuxAdditions.run
```

2. On your host (Windows/Mac/Linux), create a folder for adaptation, e.g.:

```
GSI_Adaptation/
```

3. In VirtualBox:

   * Select your VM → **Settings → Shared Folders → Add Folder**
   * Host folder path: `GSI_Adaptation/`
   * Check **Auto-mount** and **Make Permanent**

4. In Ubuntu, the folder will appear roughly as:

```bash
/media/sf_GSI_Adaptation/
```

> We will copy all AP images (`boot.img`, `vendor.img`) and extracted super.img files here.

### 5.3: Extract super.img

1. Extract the **super.img** from the GSI that caused the bootloop using `7zip` or `lpunpack`.
2. Copy from the extracted archive:

* `system.img`
* `system_ext.img` (if exists)
* `product.img` (if exists)

> ⚠️ Do **not** use `vendor.img` from the GSI author. Take it from your official AP firmware.

3. Place all files (`system.img`, `system_ext.img`, `product.img`, `vendor.img`) into the **VM shared folder** (`/media/sf_GSI_Adaptation/`).

---

### 5.4: Build super.img with lpmake

In the Ubuntu terminal, navigate to the shared folder with `system.img`, `vendor.img`, `system_ext.img`, `product.img` and run:

```bash
./lpmake \
  --metadata-size 65536 \
  --super-name super \
  --metadata-slots 2 \
  --device super:<device_size> \
  --group main:<group_size> \
  --partition system:readonly:<system_size>:main=system.img \
  --partition vendor:readonly:<vendor_size>:main=vendor.img \
  --partition system_ext:readonly:<system_ext_size>:main=system_ext.img \
  --partition product:readonly:<product_size>:main=product.img \
  --output super_new.img
```

#### 🔹 How to get file sizes for `<system_size>`, `<vendor_size>`, etc.

1. Check the exact file size in bytes:

```bash
stat -c%s system.img
stat -c%s vendor.img
stat -c%s system_ext.img
stat -c%s product.img
```

> These numbers are your `<system_size>`, `<vendor_size>`, `<system_ext_size>`, `<product_size>`.

2. If sizes are in MB or GB, convert to bytes:

```text
1 MB = 1024 * 1024 = 1,048,576 bytes
1 GB = 1024 * 1024 * 1024 = 1,073,741,824 bytes
```

**Example:**

* `system.img` = 1.7 GB → 1.7 * 1,073,741,824 ≈ 1,825,360,100 bytes
* `vendor.img` = 0.9 GB → 0.9 * 1,073,741,824 ≈ 966,367,641 bytes

3. Substitute these values into the `lpmake` command.

#### 🔹 Convert super.img to sparse (optional)

```bash
./img2simg super_new.img super_new_sparse.img
```

You will get **adapted super.img**, ready to flash with the adapted `boot.img`.

💡 **Tip:** Use the VM shared folder to easily transfer files between host and VM and avoid losing original AP and GSI images.

---

## 🔹 Step 6: Prepare for flashing

* Place `new-boot.img` and adapted `super_new.img` (or `super_new_sparse.img`) in the same folder.
* Rename `new-boot.img` → `boot.img` and `super_new.img` / `super_new_sparse.img` → `super.img`.
* Pack into a `.tar` archive for **Odin (Windows)** or flash directly via **Heimdall (Linux/Mac)**.

---

## 🔹 Step 7: Flash adapted GSI

* Follow instructions in [FLASHING_GUIDE](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/system/FLASHING_GUIDE.md) for **Odin (Windows)** or **Heimdall (Linux/Mac)**.

---

## ⚠️ Important recommendations

* Always make a **backup** before flashing.
* Ensure you use the **correct firmware** matching your Baseband / Modem.
* Follow all steps **carefully**, do not modify multiple partitions at once.
