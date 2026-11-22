# **Adaptación de LineageOS GSI para Samsung Galaxy A03 Core**

## 📱 Mantenedor no oficial: MarsXz
## GSI base por **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)**

---

## 🌍 Elige tu idioma
[English](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README.md) | **Español** | [Português](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-PT.md) | [Українська](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-UA.md) | [Русский](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-RU.md) | [Қазақша](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-KZ.md)

---

## 📺 Videotutorial

Si no quieres leer las instrucciones, puedes ver una **videoguía** detallada sobre cómo instalar y adaptar la ROM.

**[Ver la videoguía completa en YouTube](https://youtu.be/JX0434AHy6A)**

---

Este proyecto ofrece una **GSI de AOSP/LineageOS** adaptada y optimizada específicamente para el **Samsung Galaxy A03 Core**.

Está basada en **LineageOS GSI de Andy Yan**, con ajustes adicionales para mejorar la compatibilidad, la estabilidad y la experiencia general de uso.

---

## ⚠️ Descargo de responsabilidad

Instalas esta ROM **bajo tu propio riesgo**.
**No me hago responsable** de bootloops, pérdida de datos, soft-bricks, daños en el dispositivo ni cualquier otro inconveniente.

Si no estás seguro de lo que haces, **es mejor detenerse.**

---

## 📃 Documentación

**No** soy desarrollador de LineageOS ni de la GSI original.
Solo mantengo esta **adaptación no oficial** creada específicamente para el Samsung Galaxy A03 Core.

Las actualizaciones pueden incluir:

- optimizaciones específicas para el dispositivo
- correcciones de compatibilidad de kernel y vendor
- mi módulo AIO adicional de optimización
- mejoras de estabilidad

---

## 💾 Guía de instalación (Flasheo)
[Haz clic aquí para ver la guía en Español](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/FLASHING_GUIDE_ES.md)

---

## 💾 Pasos de adaptación
[Haz clic aquí para ver los pasos en Español](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/system/ADAPTATION_GUIDE_ES.md)

---

## ⭐ Características

- Optimización para **Samsung Galaxy A03 Core**
- Eliminación de componentes innecesarios que no se usan en este dispositivo
- Ajustes ocultos de Treble para una experiencia de ROM más completa
  Para activar Treble Settings:
  ```bash
  su -c "pm enable me.phh.treble.app/.TopLevelSettingsActivity"```

* **La carga con el teléfono apagado (offline charging) reinicia el dispositivo automáticamente**
*(evita que se quede congelado en el logo de Samsung en ciertos kernels)*

---

## ⛔ Problemas conocidos

* **VoLTE no es compatible**
* **Indicador incorrecto de intensidad de señal móvil**
*(siempre muestra 2 barras sin importar la señal real)*

---

## 🔧 Futuras correcciones

El autor podría lanzar en el futuro un **módulo Magisk** que potencialmente solucione parte de estos problemas.

---

## 📌 Fuentes y créditos

* **[LiteGApps](https://litegapps.github.io/)** — paquetes ligeros de GApps
* **[LineageOS](https://lineageos.org/)** — excelente ROM de código abierto
* **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)** — compilaciones de LineageOS GSI
