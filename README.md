<div align="center">

# ⚡ ADVandal

**Продвинутый перехват и уничтожение скрытых рекламных скриптов Яндекса на этапе инициализации.**

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&color=F8A307&center=true&vCenter=true&width=500&lines=Intercepting+ad+requests...;Monkey-patching+window.b...;Killing+trackers+at+root...;Saving+your+traffic!" alt="Typing SVG" />

<br>

[![GitHub license](https://img.shields.io/github/license/XPay-Company/ADVandal?style=for-the-badge&color=orange)](https://github.com/XPay-Company/ADVandal/blob/main/LICENSE)
[![uBlock Origin](https://img.shields.io/badge/uBlock%20Origin-Compatible-purple?style=for-the-badge&logo=ublockorigin)](https://github.com/gorhill/uBlock)
[![Stage](https://img.shields.io/badge/Stage-Development-blue?style=for-the-badge)](https://github.com/XPay-Company/ADVandal)

---

<p align="left">
  Обычные косметические фильтры скрывают последствия рекламы (уже загруженные баннеры). 
  <b>ADVandal работает иначе:</b> он внедряется в коллектив скриптов сайта на этапе <code>document-start</code>, замаскируется под системные объекты и ломает цепочку вызовов функций до того, как браузер отправит запрос за картинкой.
</p>

</div>

---

## 🚀 Как это работает (Под капотом)

Проект использует технику **Monkey-patching** (динамическая подмена методов «на лету»):

```mermaid
graph TD
    A[Загрузка страницы] -->|document-start| B(Инжект ADVandal)
    B --> C{Инициализация window.b}
    C -->|Перехват контроля| D[Подмена .load на noopFunc]
    C -->|Перехват контроля| E[Подмена ._formatImage]
    D --> F[Запрос заблокирован до отправки в сеть]
    E --> F
