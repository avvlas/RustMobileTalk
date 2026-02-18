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

<img src="/images/slide14_img13.png" class="mx-auto" />

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
