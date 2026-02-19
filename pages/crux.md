---
layout: section
---

# Crux - KMP на Rust

---

# Crux — что это?

<div class="grid grid-cols-2 gap-8">
<div>

- Архитектурный фреймворк + набор инструментов
- Вся бизнес-логика — в Rust **Core**
- UI — нативный: SwiftUI, Compose, React

</div>
<div>

<img src="/images/slide21_img16.png" class="scale-150" />

</div>
</div>

---

# Crux — Core + Shell

<div class="grid grid-cols-2 gap-8 items-center">
<div>

Архитектура вдохновлена **TEA** (The Elm Architecture):

<v-click>

- **Core** (Rust) — чистый, без сайд-эффектов
  - `Model` — состояние
  - `Event` — входные события
  - `update()` — логика переходов
  - `ViewModel` — данные для UI
  
</v-click>
<v-click>
  
- **Shell** (Swift/Kotlin/TS) — тонкий слой
  - Рисует UI
  - Выполняет эффекты (HTTP, KV, ...)
  - Передаёт события в Core
  
</v-click>

</div>
<div>

<img src="/images/slide22_img17.png" class="h-72 mx-auto" />

</div>
</div>

---

# Crux — биндинги и typegen

<div class="grid grid-cols-2 gap-8 items-center">
<div>

Двойной подход к FFI:

1. **UniFFI** — механизм вызова функций через FFI
2. **Кастомный typegen (serde/facet)** — генерация типов данных

<v-click>

Почему не хватает одного UniFFI?
- UniFFI не поддерживает сложные типы данных
- UniFFI не поддерживает Web
- Нужен единый подход для Swift + Kotlin + TypeScript

</v-click>

</div>
<div>

<img src="/images/slide23_img18.png" class="h-64 mx-auto" />

</div>
</div>

---

# Crux — Kotlin/Native


- В 17.0+ поддержан typegen Kotlin/Native


<img src="/images/slide24_img19.png" class="scale-90" />


---

# Crux — пример приложения

https://github.com/redbadger/crux/pull/485

---

# Crux — текущий стейт

<div class="grid grid-cols-2 gap-8">
<div>

<v-click>

**Плюсы:**
- Активно развивается
- Отзывчивое сообщество
- Web — first-class citizen
- Тесты core-логики за миллисекунды
- Compile-time safety через typegen

</v-click>

</div>
<div>

<v-click>

**Минусы:**
- Pre-1.0, API может меняться
- Мало примеров real-world приложений
- Много бойлерплейта в инфраструктурном коде
- `serde-generate` — не идеальное решение для typegen

</v-click>

</div>
</div>
