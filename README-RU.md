# **Адаптация LineageOS GSI для Samsung Galaxy A03 Core**

## 📱 Неофициальный мейнтейнер: MarsXz
## Базовый GSI от **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)**

---

## 🌍 Выберите язык
[English](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README.md) | [Український](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-UA.md) | **Русский** | [Қазақша](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-KZ.md)

---

Этот проект предоставляет адаптированный **AOSP/LineageOS GSI**, специально оптимизированный для **Samsung Galaxy A03 Core**.

В основе используется **LineageOS GSI от Andy Yan**, дополненный правками для улучшения совместимости, стабильности и общего пользовательского опыта.

---

## ⚠️ Отказ от ответственности

Вы устанавливаете прошивку **на свой страх и риск**.  
Я **не несу ответственности** за бутлупы, потерю данных, софт-брит, поломку устройства или любой другой ущерб.

Если вы не уверены в своих действиях — **лучше остановитесь.**

---

## 📃 Документация

Я **не** являюсь разработчиком LineageOS или оригинального GSI.  
Я поддерживаю только **неофициальную адаптацию** специально для Samsung Galaxy A03 Core.

Обновления могут включать:

- оптимизации под конкретное устройство  
- исправления совместимости ядра и vendor  
- мой дополнительный AIO модуль оптимизации  
- улучшения стабильности  

Особая благодарность разработчикам, перечисленным ниже.

---

## 💾 Инструкция по прошивке
[Нажмите чтобы посмотреть на Русском](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/FLASHING_GUIDE_RU.md)

---

## ⭐ Возможности

- Оптимизация под **Samsung Galaxy A03 Core**
- Удаление ненужных компонентов, не используемых на данном устройстве
- Скрытые настройки Treble для более «полного ROM-опыта»  
  Чтобы включить Treble Settings:
  ```bash
  su -c "pm enable me.phh.treble.app/.TopLevelSettingsActivity"
- **Офлайн-зарядка автоматически перезагружает устройство**  
  *(предотвращает зависание на логотипе Samsung на некоторых ядрах)*

---

## ⛔ Известные проблемы

- **VoLTE не поддерживается**
- **Неверное отображение уровня сигнала мобильной сети**  
  *(всегда показывает 2 полоски независимо от реального сигнала)*

---

## 🔧 Будущие исправления

Автор может выпустить в будущем **модуль Magisk**, который потенциально исправит часть этих проблем.

---

## 📌 Источники

- **[LiteGApps](https://litegapps.github.io/)** — пакеты облегчённых GApps  
- **[LineageOS](https://lineageos.org/)** — отличный open-source ROM  
- **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)** — сборки LineageOS GSI
