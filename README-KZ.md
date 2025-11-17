# **Samsung Galaxy A03 Core үшін LineageOS GSI бейімделуі**

## 📱 Ресми емес қолдаушы: **MarsXz**
## Негізгі GSI от **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)**

---

## 🌍 Тілді таңдаңыз
[English](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README.md) | [Український](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-UA.md) | [Русский](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-RU.md) | **Қазақша**

---

Бұл жоба **AOSP/LineageOS GSI**-ның арнайы бейімделген нұсқасын ұсынады, **Samsung Galaxy A03 Core** үшін оңтайландырылған.

Негізгі жүйе **Andy Yan жасаған LineageOS GSI**-ға сүйенеді, құрылғыға арнайы түзетулер енгізілген, бұл үйлесімділікті, тұрақтылықты және пайдаланушы тәжірибесін жақсартады.

---

## ⚠️ Жауапкершіліктен бас тарту

Бұл жүйені орнату **өз тәуекеліңізде** жүзеге асады.  
Мен **bootloop, деректерді жоғалту, soft-brick немесе құрылғы зақымдануына** жауап бермеймін.

Егер құрылғыңызды өзгертуде сенімді болмасаңыз — **тоқтаңыз.**

---

## 📃 Құжаттама

Мен **LineageOS немесе бастапқы GSI жасаушысы емеспін**.  
Мен тек **Samsung Galaxy A03 Core үшін ресми емес бейімделуді** қолдаймын.

Жаңартулар келесілерді қамтуы мүмкін:

- құрылғыға арнайы оңтайландырулар  
- ядро мен vendor үйлесімділігін түзету  
- менің қосымша AIO оптимизация модулім  
- тұрақтылықты жақсарту  

---

## 💾 Жаңарту қадамдары
[Ағылшынша көру үшін басыңыз](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/FLASHING_GUIDE_KZ.md)

---

## 💾 Адаптация қадамдары
[Ағылшынша көру үшін басыңыз](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/system/ADAPTATION_GUIDE_KZ.md)

---

## ⭐ Ерекшеліктер

- **Samsung Galaxy A03 Core** үшін арнайы оңтайландыру  
- Құрылғыда пайдаланылмайтын қажетсіз компоненттерді жою  
- Толық ROM тәжірибесі үшін Treble баптауларын жасыру  
  Treble баптауларын қосу үшін:
  ```bash
  su -c "pm enable me.phh.treble.app/.TopLevelSettingsActivity"
- **Офлайн зарядтау автоматты түрде құрылғыны қайта жүктейді**  
  *(кейбір ядроларда Samsung логотипінде қалудың алдын алады)*

---

## ⛔ Белгілі мәселелер

- **VoLTE қолдау көрсетілмейді**
- **Мобильді сигналдың дұрыс көрсетілмеуі**  
  *(нақты сигнал күшіне қарамастан әрқашан 2 жолақ көрсетіледі)*

---

## 🔧 Болашақтағы түзетулер

Автор болашақта кейбір мәселелерді түзету үшін **Magisk модулін** шығара алады.

---

## 📌 Дереккөздер

- **[LiteGApps](https://litegapps.github.io/)** — жеңіл GApps пакеттері  
- **[LineageOS](https://lineageos.org/)** — ашық кодты ROM  
- **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)** — LineageOS GSI жинақтары
