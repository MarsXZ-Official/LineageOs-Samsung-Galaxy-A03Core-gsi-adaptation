# 📘 Руководство по адаптации GSI для Samsung Galaxy A03 Core

> ℹ️ Это руководство может работать **не только на Samsung Galaxy A03 Core**,  
> но и на **других устройствах Samsung / Samsung Galaxy**,  
> если у них такая же структура разделов (`boot.img`, `vendor.img`, `super.img`)  
> и прошивки в формате AP.

Эта инструкция предназначена для **адаптации GSI** (LineageOS / AOSP), если после прошивки GSI более низкой версии на вашем устройстве произошёл **bootloop**.  

---

## ⚠️ Важно

- **Вы действуете на свой страх и риск.**  
- Автор не несёт ответственности за кирпичи, потерю данных или повреждения устройства.  
- Инструкция предназначена **только для адаптации**, если авторская версия GSI ниже вашей прошивки устройства.

> Цель адаптации: обновить `boot.img` (ядро) и `super.img` (система), чтобы GSI корректно работала с вашей прошивкой модуля связи.

---

## 🔹 Шаг 1: Определяем версию прошивки устройства

1. Перейдите в **Настройки → О телефоне / Об устройстве**.  
2. Найдите **Прошивка модуля связи / Baseband / Modem**.  
3. Эта версия критична для правильного выбора адаптационных файлов.

> Нужны два файла: `boot.img` (ядро) и `super.img` (система).

---

## 🔹 Шаг 2: Скачиваем необходимые файлы

### 2.1 Прошивка устройства

- **Для A032F**: [SM-A032F firmware](https://samfw.com/firmware/SM-A032F)  
- **Для A032M**: [SM-A032M firmware](https://samfw.com/firmware/SM-A032M)  

Скачайте **часть AP** (файл, начинающийся на `AP_...`) — в нём находится `boot.img` и `vendor.img`.

> ⚠️ Для адаптации потребуется **две версии AP**:
> 1. **AuthorVersion** — версия, под которую автор собрал GSI (или та, под которую сделана версия, вызвавшая bootloop).  
> 2. **YourVersion** — версия AP, которая установлена на вашем устройстве (совпадает с вашей прошивкой модуля связи / Baseband).

Это важно, потому что **автор GSI может не приложить boot.img**, и тогда вы обязаны взять `boot.img` из AP соответствующей версии.

### 2.2 Инструменты

- **MagiskBoot**: [скачать](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/magiskboot)  
- **lpunpack / lpmake** для Linux/Ubuntu (для сборки super.img): [скачать](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/lpunpack_and_lpmake/bin)  
- Для Windows: используем `magiskboot.exe`  

---

## 🔹 Шаг 3: Подготовка структуры папок

Создайте основную папку для адаптации, внутри две подпапки:

Adaptation/

├─ AuthorVersion/   # версия GSI, которая вызвала bootloop

└─ YourVersion/     # ваша прошивка устройства

- Скопируйте `boot.img` **из AP версии автора** (или из архива GSI, если автор приложил boot.img) в папку: `AuthorVersion/`.  
- Скопируйте ваш `boot.img` (из AP вашей прошивки) в `YourVersion/`.  
- Скопируйте `magiskboot.exe` в обе папки.  

---

## 🔹 Шаг 4: Адаптация ядра (boot.img) на Windows

1. Откройте **CMD** в каждой папке.  
2. Распакуйте `boot.img`:

```bash
magiskboot unpack boot.img
````

* В результате извлекаются **DTB и другие компоненты**.

3. Скопируйте DTB из вашей версии в папку автора и наоборот (если нужно).
4. Пересоберите `boot.img`:

```bash
magiskboot repack boot.img
```

* Получаем `new-boot.img` — адаптированное ядро.
* Держите `new-boot.img` в той же папке, где будет `super.img` для прошивки.

---

## 🔹 Шаг 5: Адаптация super.img (Linux/Ubuntu)

**Рекомендация:** используйте виртуальную машину с Linux для удобства.

1. Распакуйте **super.img** GSI, где произошёл bootloop, с помощью 7zip или `lpunpack`.
2. Скопируйте из распакованного архива:
   - `system.img`
   - `system_ext.ext` (если есть)
   - `product.ext` (если есть)

> ⚠️ `vendor.img` **не берём из GSI автора**. Его нужно взять из AP вашей официальной прошивки, которую вы скачали для вашего устройства.  

3. Поместите все файлы (`system.img`, `system_ext.img`, `product.img`, `vendor.img`) в **общую папку**.
4. Дальше собираем адаптированный `super.img` через `lpmake`.


---

### 🔹 Сборка super.img через lpmake

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

* Если нужно, преобразуйте в sparse:

```bash
./img2simg super_new.img super_new_sparse.img
```

* В результате получаем **адаптированный super.img**.

---

## 🔹 Шаг 6: Подготовка к прошивке

* Положите `new-boot.img` и адаптированный `super.img` в одну папку.
* Упакуйте в `.tar` архив для **Odin (Windows)** или прошивайте напрямую через **Heimdall (Linux/Mac)**.

---

## 🔹 Шаг 7: Прошивка адаптированного GSI

* Следуйте инструкции из [FLASHING_GUIDE_RU](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/system/FLASHING_GUIDE_RU.md) для **Odin (Windows)** или **Heimdall (Linux/Mac)**.

---

## ⚠️ Важные рекомендации

* Всегда делайте **резервную копию** перед прошивкой.
* Убедитесь, что используете **правильную прошивку**, соответствующую версии Baseband / Modem.
* Проверяйте всё **по шагам**, не меняйте сразу несколько разделов.
* Сохраняйте DTB-файлы — они пригодятся при bootloop.
