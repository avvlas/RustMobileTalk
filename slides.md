---
theme: seriph
background: https://cover.sli.dev
title: Мобильная Разработка на Rust
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Мобильная Разработка на Rust

Власюк Александр

Mobius 2026

---

# План

<div class="grid grid-cols-2 gap-8 items-center">
<div>

1. Введение в Rust
2. Примеры приложений на Rust
3. UniFFI — генерация биндингов
4. CRUX — аналог KMP на Rust
5. Кроссплатформенный UI на Rust
6. Заключение. Выводы

</div>
<div>

<img src="/images/slide2_img1.png" class="h-60 mx-auto" />

</div>
</div>

---

# Rust

Язык для системного программирования (и не только)

- **Фокусируется на производительности и безопасности**
- **Обеспечивает безопасность памяти и потоков на этапе компиляции (без Runtime и GC)**
- **"If it compiles, it will work"**
- *Имхо: Kotlin из мира системного программирования*

---

# Rust — любимый язык программистов по опросам StackOverflow

<img src="/images/slide4_img2.png" class="h-80 mx-auto" />

---

# Где используется Rust

<div class="grid grid-cols-3 gap-6">
<div>

**Systems Programming**
- Linux kernel
- Android OS
- Embedded

**CLI tools**
- Codex
- Turborepo
- Ripgrep, fd, bat, …

</div>
<div>

**Desktop & Web Apps**
- Tauri
- Dioxus

**Blockchain**
- Solana

</div>
<div>

**Mobile Development**

</div>
</div>

---

# Rust в Linux Kernel

<div class="grid grid-cols-2 gap-8 items-center">
<div>

**In December 2025 it was decided to promote Rust from experimental to a core part of the kernel.**

**This expands the core languages in the Linux kernel to C, assembly and Rust.**

</div>
<div>

<img src="/images/slide6_img3.png" class="h-60 mx-auto" />

</div>
</div>

---

# Rust в Android OS

<div class="grid grid-cols-2 gap-8 items-center">
<div>

**In Android 13, about 21% of all new native code is in Rust.**

**1.5 million total lines of Rust code in AOSP**

> "A 1000x reduction in memory safety vulnerability density compared to Android's C and C++ code."

> "With Rust changes having a 4x lower rollback rate and spending 25% less time in code review"

</div>
<div>

<img src="/images/slide7_img4.png" class="h-60 mx-auto" />

</div>
</div>

---

# Rust в мобильных приложениях

**Сценарии:**

- **Performance-critical код**
- **Использование Rust библиотек (vpn, crypto, image processing, etc)**
- **Переиспользование кода между платформами (аналог KMP)**

---

# ElementX messenger (Matrix)

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- Мессенджер на протоколе Matrix
- Matrix Rust SDK

</div>
<div>

<div class="flex gap-2">
<img src="/images/slide9_img5.png" class="h-50" />
<img src="/images/slide9_img6.png" class="h-50" />
<img src="/images/slide9_img7.png" class="h-50" />
</div>

</div>
</div>

---

# Proton Pass

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- Менеджер паролей (а также почта и т.д.)
- Common library на Rust

</div>
<div>

<div class="flex gap-2">
<img src="/images/slide10_img8.png" class="h-50" />
<img src="/images/slide10_img9.png" class="h-50" />
<img src="/images/slide10_img10.png" class="h-50" />
</div>

</div>
</div>

---

# Firefox Mobile

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- SDK на Rust
- Mozilla — компания, в которой появился Rust

</div>
<div>

<img src="/images/slide11_img11.png" class="h-70 mx-auto" />

</div>
</div>

---
layout: section
---

# UniFFI
## Вызываем Rust из других языков

---

# UniFFI

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- Проект Mozilla
- Генерация биндингов (FFI) в другие языки для Rust

</div>
<div>

<img src="/images/slide13_img12.png" class="h-60 mx-auto" />

</div>
</div>

---

# UniFFI

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- В Swift: native static library
- В Android: dynamic library (via JNA)

</div>
<div>

<img src="/images/slide14_img13.png" class="h-60 mx-auto" />

</div>
</div>

---

# UniFFI Swift

<img src="/images/slide15_img14.png" class="h-90 mx-auto" />

---

# UniFFI Kotlin

<img src="/images/slide16_img15.png" class="h-90 mx-auto" />

---

# UniFFI

Поддерживаемые языки:

<div class="grid grid-cols-2 gap-8">
<div>

**Official:**
- Swift
- Kotlin/Jvm
- Python
- Ruby

</div>
<div>

**Third Party:**
- C, C++
- C#
- Java
- Dart
- Go
- Kotlin Native

</div>
</div>

---

# UniFFI

Тут подробнее про то, как работает UniFFI

---

# UniFFI

Плагины для работы с UniFFI:

- Rust Android Gradle Plugin
- Gobley (gradle plugin для KMP проекта)

---

# UniFFI. Минусы

- Нет поддержки сложных типов (generics, etc)
- Нет интеграции с GC Jvm (нужно вызывать .destroy() на объекте)

---

# Crux

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- Кроссплатформа на Rust
- Rust Core, нативный UI

</div>
<div>

<img src="/images/slide21_img16.png" class="h-60 mx-auto" />

</div>
</div>

---

# Crux. Архитектура

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- Архитектура вдохновлена TEA
- Shell - отрисовывает UI, обрабатывает эффекты. Максимально легкий слой
- Core - описывает логику

</div>
<div>

<img src="/images/slide22_img17.png" class="h-60 mx-auto" />

</div>
</div>

---

# Crux. Биндинги

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- Использует UniFFI для генерации биндингов
- Для генерации типов используется кастомный typegen
- Для передачи объектов Rust↔Shell используется бинарная сериализация

</div>
<div>

<img src="/images/slide23_img18.png" class="h-60 mx-auto" />

</div>
</div>

---

# Crux. Kotlin

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- В текущем typegen поддержан только Kotlin/Jvm
- Мой PR с поддержкой Kotlin/Native

</div>
<div>

<img src="/images/slide24_img19.png" class="h-60 mx-auto" />

</div>
</div>

---

# Crux. Пример приложения

https://github.com/redbadger/crux/pull/485

<div class="flex gap-4 justify-center">
<img src="/images/slide25_img20.png" class="h-70" />
<img src="/images/slide25_img21.png" class="h-70" />
</div>

---

# Crux. Текущий стейт

- Активно развивается
- Отзывчивое сообщество
- Мало примеров real world приложений
- Много бойлерплейта в инфраструктурном коде (для typegen, ffi)

---

# UI на Rust. Dioxus

<img src="/images/slide27_img22.png" class="h-90 mx-auto" />

---

# UI на Rust. Dioxus

<img src="/images/slide28_img23.png" class="h-90 mx-auto" />

---

# Dioxus

- Декларативный UI на Rust
- На мобилках рендерится в WebView

---

# UI на Rust

Тут еще про Dioxus и другие решения

---
layout: center
class: text-center
---

# Выводы

выводы
