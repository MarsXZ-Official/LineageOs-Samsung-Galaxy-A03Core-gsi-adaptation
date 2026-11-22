# 📘 Guía de adaptación de GSI para Samsung Galaxy A03 Core

> ℹ️ Esta guía puede funcionar **no solo en el Samsung Galaxy A03 Core**,  
> sino también en **otros dispositivos Samsung / Samsung Galaxy**,  
> siempre que tengan la misma estructura de particiones (`boot.img`, `vendor.img`, `super.img`)  
> y firmwares en formato AP.

Estas instrucciones están diseñadas para **adaptar una GSI** (LineageOS / AOSP) si, tras flashear una GSI creada para una versión de bootloader inferior a la de tu dispositivo, ocurre un **bootloop**.

---

## ⚠️ Importante

- **Actúas bajo tu propio riesgo.**  
- El autor no se hace responsable por dispositivos brickeados, pérdida de datos o daños físicos.  
- Esta guía está destinada **únicamente a la adaptación**, cuando la versión de firmware usada por el autor de la GSI es anterior a la de tu dispositivo.

> Objetivo de la adaptación: actualizar `boot.img` (kernel) y `super.img` (sistema) para que la GSI arranque correctamente.

---

## 🔹 Paso 1: Identificar la versión de firmware del dispositivo

1. Ve a **Ajustes → Acerca del teléfono / Información del dispositivo**.  
2. Busca **Versión de banda base / Baseband / Modem**.  
3. Esta versión es fundamental para elegir los archivos de adaptación correctos.

> Necesitarás dos archivos: `boot.img` (kernel) y `super.img` (sistema).

---

## 🔹 Paso 2: Descargar los archivos necesarios

### 2.1 Firmware del dispositivo

- **Para A032F**: [Firmware SM-A032F](https://samfw.com/firmware/SM-A032F)  
- **Para A032M**: [Firmware SM-A032M](https://samfw.com/firmware/SM-A032M)  

Descarga el paquete y extrae la **parte AP** (el archivo que empieza por `AP_...`), ya que contiene `boot.img` y `super.img`.

> ⚠️ Para la adaptación necesitarás **dos versiones de AP**:
> 1. **AuthorVersion** — la versión de firmware para la cual el autor compiló la GSI (o la versión del bootloader que provocó el bootloop).  
> 2. **YourVersion** — la versión AP instalada actualmente en tu dispositivo (debe coincidir con tu versión de Banda base / Baseband).

Esto es importante porque **el autor de la GSI puede no incluir `boot.img`**, en cuyo caso deberás extraerlo del archivo AP correspondiente.

### 2.2 Herramientas

- **MagiskBoot**: [descargar](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/magiskboot)  
- **lpunpack / lpmake** para Linux/Ubuntu (para compilar `super.img`): [descargar](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/Adaptation/lpunpack_and_lpmake/bin)  
- Para Windows: usamos `magiskboot.exe`  

---

## 🔹 Paso 3: Preparar la estructura de carpetas

Crea una carpeta principal para la adaptación con dos subcarpetas dentro:

```text
Adaptation/
├─ AuthorVersion/   # versión de la GSI que provocó el bootloop
└─ YourVersion/     # firmware actual de tu dispositivo

```

* Copia el `boot.img` **del AP de la versión del autor** (o del archivo de la GSI, si el autor lo incluyó) en la carpeta `AuthorVersion/`.
* Copia tu `boot.img` (extraído del AP de tu firmware) en `YourVersion/`.
* Copia `magiskboot.exe` en ambas carpetas.

---

## 🔹 Paso 4: Adaptar el kernel (`boot.img`) en Windows

1. Abre **CMD** (Símbolo del sistema) dentro de cada carpeta.
2. Desempaqueta `boot.img`:

```bash
magiskboot unpack boot.img

```

* Esto extraerá el archivo **dtb y otros componentes**.

3. Copia el archivo `dtb` de tu carpeta (`YourVersion`) hacia la carpeta del autor (`AuthorVersion`) reemplazándolo.
4. Reempaqueta `boot.img`:

```bash
magiskboot repack boot.img

```

* Obtendrás `new-boot.img` — el kernel ya adaptado.
* Guarda `new-boot.img` en la misma carpeta donde prepararás `super.img` para flashear.

---

## 🔹 Paso 5: Adaptar `super.img` (Linux/Ubuntu)

**Recomendación:** utiliza una máquina virtual con Linux (Ubuntu) por comodidad y seguridad.

### 5.1: Instalar VirtualBox y Ubuntu

1. Descarga e instala [VirtualBox](https://www.virtualbox.org/).
2. Instala Ubuntu en VirtualBox (puedes usar una versión LTS reciente, como 24.04 LTS).

### 5.2: Configurar carpetas compartidas

Para que la máquina virtual pueda acceder a los archivos de tu equipo anfitrión:

1. Instala **Guest Additions** en Ubuntu:

```bash
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)
# luego en VirtualBox: Dispositivos → Insertar imagen de CD de las «Guest Additions»
sudo sh /media/<punto_montaje_cd>/VBoxLinuxAdditions.run

```

2. En tu equipo anfitrión (Windows/Mac/Linux), crea una carpeta de trabajo, por ejemplo:

```text
GSI_Adaptation/

```

3. En VirtualBox:

* Selecciona tu VM → **Configuración → Carpetas compartidas → Añadir carpeta**
* Ruta de la carpeta en el anfitrión: `GSI_Adaptation/`
* Marca **Automontar** y **Hacer permanente**

4. En Ubuntu, la carpeta estará disponible en una ruta similar a:

```bash
/media/sf_GSI_Adaptation/

```

> En esta carpeta copiaremos todas las imágenes del AP (`boot.img`, `vendor.img`) y los archivos extraídos de `super.img`.

### 5.3: Desempaquetar `super.img`

1. Desempaqueta el **`super.img`** de la GSI que dio bootloop usando `7zip` o `lpunpack`.
2. Copia desde los archivos extraídos:

* `system.img`
* `system_ext.ext` (si existe)
* `product.ext` (si existe)

> ⚠️ **No tomes `vendor.img` de la GSI del autor.** Extráelo del AP de tu firmware oficial descargado para tu dispositivo.

3. Coloca todos los archivos (`system.img`, `system_ext.ext`, `product.ext`, `vendor.img`) en la **carpeta compartida de VirtualBox** (`/media/sf_GSI_Adaptation/` o desde el anfitrión).

### 5.4: Compilar `super.img` con `lpmake`

En la terminal de Ubuntu, ve a la carpeta compartida con los archivos (`system.img`, `vendor.img`, `system_ext.ext`, `product.ext`) y ejecuta `lpmake` para construir el nuevo `super.img`:

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

#### 🔹 Cómo obtener el tamaño de los archivos para `<system_size>`, `<vendor_size>`, etc.

1. Consulta el tamaño exacto de cada archivo en bytes con el comando:

```bash
stat -c%s system.img
stat -c%s vendor.img
stat -c%s system_ext.ext
stat -c%s product.ext

```

> Estos números corresponden a `<system_size>`, `<vendor_size>`, `<system_ext_size>` y `<product_size>`.

2. Si los tamaños aparecen en megabytes (MB) o gigabytes (GB), conviértelos a bytes:

```text
1 MB = 1024 * 1024 = 1 048 576 bytes
1 GB = 1024 * 1024 * 1024 = 1 073 741 824 bytes

```

**Ejemplo:**

* `system.img` = 1.7 GB → 1.7 * 1 073 741 824 ≈ 1 825 360 100 bytes
* `vendor.img` = 0.9 GB → 0.9 * 1 073 741 824 ≈ 966 367 641 bytes

3. Sustituye los valores obtenidos en el comando `lpmake`.

#### 🔹 Convertir `super.img` a formato sparse (opcional)

```bash
./img2simg super_new.img super_new_sparse.img

```

Como resultado obtendrás un **`super.img` adaptado**, listo para flashearse junto con el `boot.img` adaptado.

💡 **Consejo:** usa la carpeta compartida de la máquina virtual para mover archivos fácilmente entre el anfitrión y Ubuntu sin perder las imágenes originales.

---

## 🔹 Paso 6: Preparación para el flasheo

* Coloca `new-boot.img` y el archivo adaptado `super_new.img` (o `super_new_sparse.img`) en una misma carpeta.
* Renombra `new-boot.img` → `boot.img`, y `super_new.img` / `super_new_sparse.img` → `super.img`.
* Empaqueta los archivos en un archivo `.tar` para **Odin (Windows)** o flashea directamente mediante **Heimdall (Linux/Mac)**.

---

## 🔹 Paso 7: Flashear la GSI adaptada

* Sigue las instrucciones de [FLASHING_GUIDE_ES](https://www.google.com/search?q=https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/system/FLASHING_GUIDE_ES.md) para **Odin (Windows)** o **Heimdall (Linux/Mac)**.

---

## ⚠️ Recomendaciones importantes

* Haz siempre una **copia de seguridad** antes de flashear.
* Asegúrate de usar el **firmware correcto** que coincida con tu versión de Baseband / Modem.
* Verifica todo **paso a paso** y no modifiques varias particiones innecesarias a la vez.
