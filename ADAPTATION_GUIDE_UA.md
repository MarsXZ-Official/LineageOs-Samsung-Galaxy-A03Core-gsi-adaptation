# 📘 Посібник з адаптації GSI для Samsung Galaxy A03 Core

> ℹ️ Цей посібник працює **не лише на Samsung Galaxy A03 Core**,  
> а й на **інших пристроях Samsung / Samsung Galaxy**,  
> якщо у них така ж структура розділів (`boot.img`, `vendor.img`, `super.img`)  
> і прошивки у форматі AP.

Цей посібник призначений для **адаптації GSI** (LineageOS / AOSP), якщо встановлення нижчої версії GSI на вашому пристрої викликає **bootloop**.

---

## ⚠️ Важливо

- **Ви дієте на власний ризик.**  
- Автор не несе відповідальності за «цеглу» пристрою, втрату даних або пошкодження.  
- Посібник призначений **тільки для адаптації**, якщо версія GSI автора нижча за вашу прошивку.

> Мета адаптації: оновити `boot.img` (ядро) і `super.img` (система), щоб GSI працювала правильно.

---

## 🔹 Крок 1: Перевірка версії прошивки пристрою

1. Перейдіть у **Налаштування → Про телефон / Про пристрій**.  
2. Знайдіть **версію Baseband / Modem**.  
3. Ця версія критично важлива для вибору правильних файлів адаптації.

> Потрібні два файли: `boot.img` (ядро) і `super.img` (система).

---

## 🔹 Крок 2: Завантаження необхідних файлів

### 2.1 Фірмова прошивка пристрою

- **Для A032F**: [SM-A032F firmware](https://samfw.com/firmware/SM-A032F)  
- **Для A032M**: [SM-A032M firmware](https://samfw.com/firmware/SM-A032M)  

Завантажте **частину AP** (файл, що починається з `AP_...`) — у ній містяться `boot.img` та `super.img`.

> ⚠️ Для адаптації знадобляться **дві версії AP**:
> 1. **AuthorVersion** — версія AP, яку використовував автор GSI (або та, що викликала bootloop).  
> 2. **YourVersion** — версія AP, встановлена на вашому пристрої (відповідає Baseband / Modem).

Це важливо, оскільки **автор GSI може не додати boot.img**, тоді його потрібно взяти з відповідної версії AP.

### 2.2 Інструменти

- **MagiskBoot**: [завантажити](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/magiskboot)  
- **lpunpack / lpmake** для Linux/Ubuntu (для складання super.img): [завантажити](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/lpunpack_and_lpmake/bin)  
- У Windows використовуйте `magiskboot.exe`  

---

## 🔹 Крок 3: Підготовка структури папок

Створіть основну папку для адаптації з двома підпапками:

Adaptation/

├─ AuthorVersion/   # версія GSI, що викликала bootloop

└─ YourVersion/     # ваша фірмова прошивка

- Скопіюйте `boot.img` **з AuthorVersion AP** (або з архіву GSI, якщо надано) → `AuthorVersion/`.  
- Скопіюйте ваш `boot.img` (з вашої AP прошивки) → `YourVersion/`.  
- Скопіюйте `magiskboot.exe` у обидві папки.  

---

## 🔹 Крок 4: Адаптація boot.img у Windows

1. Відкрийте **CMD** у кожній папці.  
2. Розпакуйте `boot.img`:

```bash
magiskboot unpack boot.img
````

* Це витягує **dtb та інші компоненти**.

3. Скопіюйте DTB із вашої версії до папки автора і навпаки (за потреби).
4. Перекомпонуйте `boot.img`:

```bash
magiskboot repack boot.img
```

* Отримуєте `new-boot.img` — адаптоване ядро.
* Залиште `new-boot.img` у тій самій папці, де буде `super.img` для прошивки.

---

## 🔹 Крок 5: Адаптація super.img (Linux/Ubuntu)

**Рекомендація:** використовуйте віртуальну машину Linux (Ubuntu) для зручності та безпеки.

### 5.1: Встановлення VirtualBox та Ubuntu

1. Завантажте і встановіть [VirtualBox](https://www.virtualbox.org/).
2. Встановіть Ubuntu у VirtualBox (остання LTS версія, наприклад 24.04 LTS).

### 5.2: Налаштування спільних папок

Щоб VM могла бачити файли на хості:

1. Встановіть **Guest Additions** у Ubuntu:

```bash
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)
# У VirtualBox: Devices → Insert Guest Additions CD → ISO монтовано
sudo sh /media/<cdrom_mount>/VBoxLinuxAdditions.run
```

2. На хості (Windows/Mac/Linux) створіть папку для адаптації, наприклад:

```
GSI_Adaptation/
```

3. У VirtualBox:

   * Виберіть VM → **Settings → Shared Folders → Add Folder**
   * Шлях до папки на хості: `GSI_Adaptation/`
   * Позначте **Auto-mount** і **Make Permanent**

4. В Ubuntu папка буде приблизно тут:

```bash
/media/sf_GSI_Adaptation/
```

> Тут ми будемо копіювати всі AP образи (`boot.img`, `vendor.img`) та витягнуті файли super.img.

### 5.3: Розпакування super.img

1. Розпакуйте **super.img** з GSI, що викликала bootloop, за допомогою `7zip` або `lpunpack`.
2. Скопіюйте з архіву:

* `system.img`
* `system_ext.ext` (якщо є)
* `product.ext` (якщо є)

> ⚠️ Не беріть `vendor.img` від автора GSI. Використовуйте його з офіційної AP прошивки.

3. Помістіть усі файли (`system.img`, `system_ext.ext`, `product.ext`, `vendor.img`) у **спільну папку VM** (`/media/sf_GSI_Adaptation/`).

---

### 5.4: Створення super.img за допомогою lpmake

В терміналі Ubuntu перейдіть у спільну папку та виконайте:

```bash
./lpmake \
  --metadata-size 65536 \
  --super-name super \
  --metadata-slots 2 \
  --device super:<device_size> \
  --group main:<group_size> \
  --partition system:readonly:<system_size>:main=system.img \
  --partition vendor:readonly:<vendor_size>:main=vendor.img \
  --partition system_ext:readonly:<system_ext_size>:main=system_ext.ext \
  --partition product:readonly:<product_size>:main=product.ext \
  --output super_new.img
```

#### 🔹 Як дізнатися розміри файлів для `<system_size>`, `<vendor_size>` тощо

1. Дізнайтесь точний розмір файлу в байтах:

```bash
stat -c%s system.img
stat -c%s vendor.img
stat -c%s system_ext.ext
stat -c%s product.ext
```

> Це і є ваші `<system_size>`, `<vendor_size>`, `<system_ext_size>`, `<product_size>`.

2. Якщо розмір у MB або GB, переведіть у байти:

```text
1 MB = 1024 * 1024 = 1,048,576 байт
1 GB = 1024 * 1024 * 1024 = 1,073,741,824 байт
```

**Приклад:**

* `system.img` = 1.7 GB → 1.7 * 1,073,741,824 ≈ 1,825,360,100 байт
* `vendor.img` = 0.9 GB → 0.9 * 1,073,741,824 ≈ 966,367,641 байт

3. Підставте ці значення у команду `lpmake`.

#### 🔹 Конвертація super.img у sparse (за бажанням)

```bash
./img2simg super_new.img super_new_sparse.img
```

Отримаєте **адаптований super.img**, готовий до прошивки разом із `boot.img`.

💡 **Порада:** використовуйте спільну папку VM для зручного перенесення файлів між хостом і VM та щоб не втратити оригінальні AP і GSI образи.

---

## 🔹 Крок 6: Підготовка до прошивки

* Помістіть `new-boot.img` і адаптований `super_new.img` (або `super_new_sparse.img`) в одну папку.
* Перейменуйте `new-boot.img` → `boot.img`, а `super_new.img` / `super_new_sparse.img` → `super.img`.
* Запакуйте в `.tar` для **Odin (Windows)** або прошивайте безпосередньо через **Heimdall (Linux/Mac)**.

---

## 🔹 Крок 7: Прошивка адаптованого GSI

* Дотримуйтесь інструкцій у [FLASHING_GUIDE_UA](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/system/FLASHING_GUIDE_UA.md) для **Odin (Windows)** або **Heimdall (Linux/Mac)**.

---

## ⚠️ Важливі рекомендації

* Завжди робіть **резервну копію** перед прошивкою.
* Переконайтесь, що використовуєте **правильну прошивку**, що відповідає вашій Baseband / Modem версії.
* Виконуйте всі кроки **уважно**, не змінюйте кілька розділів одночасно.
