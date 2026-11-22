# 📘 GUÍA COMPLETA PARA FLASHEAR GSI CON ODIN (Windows), ODIN4 (Linux) Y HEIMDALL (Mac)

Esta guía es válida para todos los dispositivos Samsung con bootloader desbloqueado y soporte para Project Treble (GSI).

---

> ⚠️ **Descargo de responsabilidad**
> 
> **Realizas el flasheo bajo tu propio riesgo. El autor de esta guía no se hace responsable por daños en el dispositivo, pérdida de datos o fallos de funcionamiento.**

---

## ✅ 1. Requisitos

- Dispositivo Samsung con el bootloader desbloqueado
- PC con Windows, Linux o macOS
- Odin (para Windows / Linux) o Heimdall (para macOS)
- Drivers USB de Samsung (para Windows)
- Un cable USB de buena calidad
- **Archivo con `super.img`** (y `boot.img`, si está incluido) — la GSI descargada
- Recomendado: batería cargada al menos al 80%

> ⚠️ **Atención:** Al descargar la GSI obtendrás un **archivo comprimido**. **No necesitas extraerlo por completo** — los pasos dependen de tu sistema operativo:
> * **En Windows / Linux (Odin / Odin4)**: flashea el archivo `LineageOs20/21.tar` tal como está.
> * **En macOS (Heimdall)**: extrae `LineageOs20/21.tar` para obtener `boot.img` (si existe) y `super.img` — esos son los archivos que debes flashear.

---

## 🧰 2. Preparación del dispositivo

### 2.1. Activar las opciones de desarrollador

1. Ve a **Ajustes → Acerca del teléfono**.
2. Pulsa 7 veces sobre **Número de compilación (Build number)**.
3. Regresa atrás, entra en **Opciones de desarrollador** y activa:
   - **Desbloqueo OEM (OEM Unlocking)**
   - **Depuración por USB (USB Debugging)**

### 2.2. Desbloquear el bootloader

> Asegúrate de tener al menos un 80% de batería.  
> **¡NO conectes el cable USB todavía!** Si está conectado, desconéctalo; de lo contrario el teléfono entrará en modo carga.

1. Apaga completamente el teléfono y espera 10 segundos.
2. Mantén presionados al mismo tiempo **Bajar volumen + Encendido**.
3. Cuando aparezca el logotipo de Samsung, suelta el botón de **Encendido**, pero sigue manteniendo **Bajar volumen** hasta que aparezca una pantalla azul de advertencia.
4. **Conecta el cable USB** al PC.
5. Realiza una **PULSACIÓN LARGA en Subir volumen** para entrar al modo de desbloqueo (Device unlock mode).
6. Pulsa **Subir volumen** una vez para confirmar.
7. El dispositivo borrará todos los datos, desbloqueará el bootloader y se reiniciará automáticamente.

### 2.3. Borrado de datos (Factory Reset)

*(Espera a que el sistema inicie por completo tras el desbloqueo y vuelve a apagar el teléfono).*

1. Entra al modo Recovery (mantén presionados **Subir volumen + Encendido** al encender).
2. Selecciona **Wipe data / factory reset**.
3. Elige **Factory data reset** para confirmar.

### 2.4. Entrar en Download Mode

1. Apaga el dispositivo.
2. Mantén presionados **Bajar volumen + Subir volumen** y **conecta el cable USB**.
3. Cuando aparezca la pantalla azul de advertencia, suelta los botones.
4. Haz una **PULSACIÓN CORTA en Subir volumen** para entrar en Download Mode y comenzar el flasheo.

---

## 🪟 3. Flasheo mediante Odin (Windows)

### 3.1. Descarga e instalación

- [Descargar Odin (se recomienda la versión 3.14.4)](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/tree/system/%D0%9Edin)
- [Instalar Samsung USB Drivers](https://developer.samsung.com/android-usb-driver)

### 3.2. Configuración de Odin

1. Abre Odin.
2. Conecta el dispositivo → en el registro (Log) debe aparecer el mensaje **Added!!**
3. Haz clic en el botón **AP** → selecciona el **archivo en formato `.tar`**.
4. En la pestaña **Options**, asegúrate de que estén marcadas **únicamente**:
   - **Auto Reboot**
   - **F. Reset Time**

> ❌ **NO marques:** Re-Partition ni Nand Erase.

### 3.3. Iniciar el flasheo

Haz clic en **Start** y espera a que finalice el proceso (normalmente tarda entre 1 y 3 minutos).

Si todo salió bien, la casilla superior izquierda se pondrá verde con el mensaje **PASS!**

### 3.4. Verificar el resultado

El teléfono se reiniciará automáticamente.

- Si el sistema inicia — la GSI se ha instalado correctamente.
- Si el dispositivo entra en bootloop (reinicio infinito) — entra al Recovery y realiza un Wipe Data.

---

## 🐧 4. Flasheo mediante Odin4 (Linux)

### 4.1. Descarga y configuración

1. **Descarga Odin4:**  
   Ve a la [página del repositorio](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/system/%D0%9Edin/Linux/odin4) y descarga el archivo `odin4`.
2. **Dale permisos de ejecución a Odin4:**  
   Cambia los permisos del binario desde la terminal:
   ```bash
   chmod +x odin4```

3. **Mueve `odin4` al directorio `/bin`:**
Para poder ejecutarlo desde cualquier ruta del sistema:
```bash
sudo cp odin4 /bin

```



### 4.2. Proceso de flasheo

Ejecuta el flasheo con el siguiente comando:

```bash
sudo odin4 -a nombre_del_archivo.tar --reboot

```

**Explicación del comando:**

* `sudo` — ejecuta el comando con permisos de administrador (root).
* `odin4` — invoca el programa.
* `-a` — especifica la partición a flashear (AP).
* `nombre_del_archivo.tar` — ruta de tu archivo `.tar`.
* `--reboot` — reinicia el dispositivo automáticamente al terminar.

---

## 🍏 5. Flasheo mediante Heimdall (macOS)

### 5.1. Instalar Heimdall

Abre la terminal e instala Heimdall usando Homebrew:

```bash
brew install heimdall

```

### 5.2. Preparar los archivos

1. Descarga el archivo `.tar` de la GSI.
2. Extráelo desde la terminal:
```bash
tar -xf nombre_del_archivo.tar

```


3. Obtendrás uno o dos archivos: `boot.img` (si está incluido) y `super.img`.
4. Entra en la carpeta con los archivos extraídos:
```bash
cd ~/ruta_de_los_archivos_extraidos

```



### 5.3. Comprobar la conexión

Asegúrate de que el teléfono esté en **Download Mode** y conectado al Mac, luego ejecuta:

```bash
heimdall detect

```

Si la terminal muestra *Device detected*, el dispositivo está listo para flashear.

### 5.4. Flasheo

Flashea la partición `super`:

```bash
sudo heimdall flash --super super.img

```

> **Nota:** En algunos dispositivos Samsung la partición puede llamarse `--super` o `--SUPER`. Si aparece un error, prueba cambiando mayúsculas/minúsculas.

Si el archivo `.tar` incluía `boot.img`, flashea también esa partición:

```bash
sudo heimdall flash --boot boot.img

```

### 5.5. Finalización y comprobación

Heimdall mostrará el progreso y reiniciará el teléfono automáticamente.

* El dispositivo inicia → todo salió correctamente.
* El dispositivo entra en bootloop → entra en Recovery y haz un Wipe Data.
* Error de flasheo → verifica la compatibilidad de la GSI descargada con tu modelo y binario.

---

## 🎉 ¡Listo!

**La GSI ya está instalada y lista para usarse.**

**Agora a GSI está instalada e pronta para uso.**
