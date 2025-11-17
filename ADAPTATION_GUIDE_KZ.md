# 📘 Samsung Galaxy A03 Core үшін GSI бейімдеу нұсқаулығы

> ℹ️ Бұл нұсқаулық тек **Samsung Galaxy A03 Core** үшін ғана емес,  
> **басқа Samsung / Samsung Galaxy құрылғыларына** да жарайды,  
> егер олардың бөлім құрылымы (`boot.img`, `vendor.img`, `super.img`) бірдей болса  
> және AP форматындағы фирмалық бағдарламалар қолданылса.

Бұл нұсқаулық **GSI бейімдеуге** арналған (LineageOS / AOSP), егер құрылғыңызға төменгі нұсқалы GSI орнатқанда **bootloop** орын алса.

---

## ⚠️ Маңызды

- **Барлығы өз тәуекеліңізде орындалады.**  
- Автор құрылғының "кирпич" болуы, деректердің жоғалуы немесе зақымдалуы үшін жауап бермейді.  
- Бұл нұсқаулық тек **бейімдеу үшін** арналған, егер автордың GSI нұсқасы сіздің құрылғыңыздың фирмалық бағдарламасынан төмен болса.

> Бейімдеудің мақсаты: `boot.img` (ядро) және `super.img` (жүйе) жаңартып, GSI дұрыс жұмыс істеуін қамтамасыз ету.

---

## 🔹 1-қадам: Құрылғының фирмалық бағдарламасының нұсқасын тексеру

1. **Параметрлер → Құрылғы туралы / Телефон туралы** бөліміне өтіңіз.  
2. **Baseband / Modem нұсқасын** табыңыз.  
3. Бұл нұсқа бейімдеу файлдарын дұрыс таңдау үшін өте маңызды.

> Сізге екі файл қажет: `boot.img` (ядро) және `super.img` (жүйе).

---

## 🔹 2-қадам: Қажетті файлдарды жүктеу

### 2.1 Құрылғының фирмалық бағдарламасы

- **A032F үшін**: [SM-A032F firmware](https://samfw.com/firmware/SM-A032F)  
- **A032M үшін**: [SM-A032M firmware](https://samfw.com/firmware/SM-A032M)  

**AP бөлігін** жүктеңіз (файл `AP_...` деп басталады) — онда `boot.img` және `super.img` бар.

> ⚠️ Бейімдеу үшін сізге **екі AP нұсқасы** қажет:
> 1. **AuthorVersion** — GSI авторы қолданған AP нұсқасы (немесе bootloop туғызған нұсқа).  
> 2. **YourVersion** — құрылғыңызда орнатылған AP нұсқасы (Baseband / Modem нұсқасына сәйкес).

Бұл маңызды, себебі **GSI авторы boot.img қоса бермеуі мүмкін**, сонда сізге сәйкес AP нұсқасынан `boot.img` алу қажет.

### 2.2 Құралдар

- **MagiskBoot**: [жүктеу](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/magiskboot)  
- **lpunpack / lpmake** Linux/Ubuntu үшін (super.img құрастыруға): [жүктеу](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/lpunpack_and_lpmake/bin)  
- Windows үшін: `magiskboot.exe` пайдаланыңыз  

---

## 🔹 3-қадам: Қапшық құрылымын дайындау

Бейімдеу үшін негізгі қапшық жасаңыз, ішінде екі ішкі қапшық:

Adaptation/

├─ AuthorVersion/   # bootloop туғызған GSI нұсқасы

└─ YourVersion/     # сіздің құрылғының фирмалық бағдарламасы

- `boot.img` **AuthorVersion AP-тен** көшіріңіз (немесе GSI архивінен, егер берілсе) → `AuthorVersion/`.  
- Сіздің `boot.img` (сіздің AP фирмалық бағдарламасынан) → `YourVersion/`.  
- `magiskboot.exe` екі қапшыққа да көшіріңіз.  

---

## 🔹 4-қадам: Windows-та boot.img бейімдеу

1. Әр қапшықта **CMD** ашыңыз.  
2. `boot.img` файлды ашыңыз:

```bash
magiskboot unpack boot.img
````

* Бұл **dtb және басқа компоненттерді** шығарады.

3. DTB файлын өз нұсқадан автордың қапшығына және керісінше көшіріңіз (қажет болса).
4. `boot.img` қайта құраңыз:

```bash
magiskboot repack boot.img
```

* `new-boot.img` — бейімделген ядро дайын болады.
* `new-boot.img` файлды `super.img` орнатылатын сол қапшықта сақтаңыз.

---

## 🔹 5-қадам: super.img бейімдеу (Linux/Ubuntu)

**Ұсыныс:** ыңғайлы және қауіпсіз болу үшін Linux виртуалды машинасын (Ubuntu) қолданыңыз.

### 5.1: VirtualBox және Ubuntu орнату

1. [VirtualBox](https://www.virtualbox.org/) жүктеп, орнатыңыз.
2. Ubuntu орнатыңыз (ең соңғы LTS нұсқасы, мысалы 24.04 LTS).

### 5.2: Ортақ қапшықтарды баптау

VM хосттағы файлдарға қол жеткізе алуы үшін:

1. Ubuntu-да **Guest Additions** орнатыңыз:

```bash
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)
# VirtualBox-та: Devices → Insert Guest Additions CD → ISO монтинг
sudo sh /media/<cdrom_mount>/VBoxLinuxAdditions.run
```

2. Хостта (Windows/Mac/Linux) бейімдеу үшін қапшық жасаңыз, мысалы:

```
GSI_Adaptation/
```

3. VirtualBox-та:

   * VM таңдаңыз → **Settings → Shared Folders → Add Folder**
   * Host қапшық жолы: `GSI_Adaptation/`
   * **Auto-mount** және **Make Permanent** таңдаңыз

4. Ubuntu ішінде қапшық шамамен осылай көрінеді:

```bash
/media/sf_GSI_Adaptation/
```

> Барлық AP суреттерін (`boot.img`, `vendor.img`) және super.img файлдарын осы жерге көшіресіз.

### 5.3: super.img ашу

1. Bootloop туғызған GSI-дегі **super.img** файлын `7zip` немесе `lpunpack` арқылы ашыңыз.
2. Ашу архивінен көшіріңіз:

* `system.img`
* `system_ext.ext` (бар болса)
* `product.ext` (бар болса)

> ⚠️ GSI авторының `vendor.img` файлын қолданбаңыз. Сіздің ресми AP фирмалық бағдарламасынан алыңыз.

3. Барлық файлдарды (`system.img`, `system_ext.ext`, `product.ext`, `vendor.img`) **VM ортақ қапшығына** орналастырыңыз (`/media/sf_GSI_Adaptation/`).

---

### 5.4: lpmake арқылы super.img құру

Ubuntu терминалында ортақ қапшыққа өтіп, `system.img`, `vendor.img`, `system_ext.ext`, `product.ext` файлдарымен іске қосыңыз:

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

#### 🔹 `<system_size>`, `<vendor_size>` және т.б. өлшемін анықтау

1. Файлдың нақты өлшемін байтпен қараңыз:

```bash
stat -c%s system.img
stat -c%s vendor.img
stat -c%s system_ext.ext
stat -c%s product.ext
```

> Бұл сандар сіздің `<system_size>`, `<vendor_size>`, `<system_ext_size>`, `<product_size>` болады.

2. Егер өлшем MB немесе GB-де болса, оны байтқа ауыстырыңыз:

```text
1 MB = 1024 * 1024 = 1,048,576 байт
1 GB = 1024 * 1024 * 1024 = 1,073,741,824 байт
```

**Мысал:**

* `system.img` = 1.7 GB → 1.7 * 1,073,741,824 ≈ 1,825,360,100 байт
* `vendor.img` = 0.9 GB → 0.9 * 1,073,741,824 ≈ 966,367,641 байт

3. Осы мәндерді `lpmake` командасына қойыңыз.

#### 🔹 super.img-ды sparse форматына ауыстыру (қалауы бойынша)

```bash
./img2simg super_new.img super_new_sparse.img
```

Сіз **бейімделген super.img** аласыз, оны `boot.img` файлымен бірге флештеуге болады.

💡 **Кеңес:** VM ортақ қапшықты қолданыңыз, файлдарды хост пен VM арасында оңай тасымалдау үшін және бастапқы AP және GSI суреттерін жоғалтпау үшін.

---

## 🔹 6-қадам: Флешке дайындау

* `new-boot.img` және бейімделген `super_new.img` (немесе `super_new_sparse.img`) бір қапшыққа орналастырыңыз.
* `new-boot.img` → `boot.img`, ал `super_new.img` / `super_new_sparse.img` → `super.img` деп өзгертіңіз.
* `.tar` архивіне орап **Odin (Windows)** арқылы немесе **Heimdall (Linux/Mac)** арқылы тікелей флештеңіз.

---

## 🔹 7-қадам: Бейімделген GSI орнату

* [FLASHING_GUIDE_KZ](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/system/FLASHING_GUIDE_KZ.md) нұсқаулығына сәйкес **Odin (Windows)** немесе **Heimdall (Linux/Mac)** арқылы орнатыңыз.

---

## ⚠️ Маңызды ұсыныстар

* Флешке қою алдында әрқашан **резервтік көшірме** жасаңыз.
* Baseband / Modem нұсқасына сәйкес **дұрыс фирмалық бағдарламаны** пайдаланыңыз.
* Барлық қадамдарды **мұқият орындаңыз**, бірнеше бөлімді бірден өзгертпеңіз.
