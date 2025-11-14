 # **Адаптація LineageOS GSI для Samsung Galaxy A03 Core**

## 📱 Неофіційний мейнтейнер: **MarsXz**
## Базовий GSI від **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)**

---

## 🌍 Оберіть мову
[English](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README.md) | [Русский](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-RU.md) | [Қазақша](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-KZ.md) | **Український**

---

Цей проєкт надає адаптовану версію **AOSP/LineageOS GSI**, спеціально оптимізовану для **Samsung Galaxy A03 Core**.

Базова система використовує **LineageOS GSI від Andy Yan**, з пристроєвими змінами для покращення сумісності, стабільності та загального користувацького досвіду.

---

## ⚠️ Відмова від відповідальності

Ви встановлюєте прошивку **на свій страх і ризик**.  
Я **не несу відповідальності** за bootloop, втрату даних, soft-brick або будь-які пошкодження пристрою.

Якщо ви не впевнені у своїх діях — **краще зупиніться**.

---

## 📃 Документація

Я **не** є розробником LineageOS або оригінального GSI.  
Я підтримую лише **неофіційну адаптацію** спеціально для Samsung Galaxy A03 Core.

Оновлення можуть включати:

- оптимізації під конкретний пристрій  
- виправлення сумісності ядра та vendor  
- мій додатковий AIO модуль оптимізації  
- покращення стабільності  

Особлива подяка розробникам, зазначеним нижче.

---

## 💾 Кроки прошивки
*(Будуть додані пізніше — якщо бажаєте, можу написати повний гайд.)*

---

## ⭐ Можливості

- Оптимізація під **Samsung Galaxy A03 Core**
- Видалення непотрібних компонентів, що не використовуються на пристрої
- Приховані налаштування Treble для більш повного **ROM-досвіду**  
  Щоб увімкнути Treble Settings:
  ```bash
  su -c "pm enable me.phh.treble.app/.TopLevelSettingsActivity"
- **Офлайн-зарядка автоматично перезавантажує пристрій**  
  *(запобігає зависанню на логотипі Samsung на деяких ядрах)*

---

## ⛔ Відомі проблеми

- **VoLTE не підтримується**
- **Неправильне відображення рівня мобільного сигналу**  
  *(завжди показує 2 смужки незалежно від реальної сили сигналу)*

---

## 🔧 Майбутні виправлення

Автор може в майбутньому випустити **Magisk-модуль**, який потенційно виправить деякі з цих проблем.

---

## 📌 Джерела

- **[LiteGApps](https://litegapps.github.io/)** — легкі пакети GApps  
- **[LineageOS](https://lineageos.org/)** — відмінний open-source ROM  
- **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)** — збірки LineageOS GSI
