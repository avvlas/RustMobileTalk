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

# Crux — поддержка Kotlin/Native

https://github.com/redbadger/facet-generate/pull/48  
https://github.com/zefchain/serde-reflection/pull/86

<img src="/images/SerdeGenerateKotlin.png" class="scale-75" />


---

# Crux — Android пример

https://github.com/redbadger/crux/pull/485

<img src="/images/CruxAndroidExample.png" class="scale-75" />

---

# Crux в Photoroom

<div class="flex h-full flex-col">

- Редактор фотографий для Android / iOS / WASM
- Engine на CRUX, нативный UI
 
<img src="/images/Photoroom.webp" class="scale-75" />

<!--<a
  href="https://www.photoroom.com/inside-photoroom/building-live-collaboration-in-rust-for-millions-of-users-part-1"
  class="mt-2 text-xs leading-tight"
>
  photoroom.com/inside-photoroom/building-live-collaboration-in-rust-for-millions-of-users-part-1
</a>-->

</div>

---

# Crux в Proton

- Набор open-source утилит-приложений
- Заадоптили CRUX, рассказали на RustNation: https://github.com/ProtonMail/proton-rust-nation-2026

<img src="/images/ProtonFamily.webp" class="scale-60 -mt-16" />

---

# Crux — текущий стейт

<div class="grid grid-cols-2 gap-8">
<div>

<v-click>

**Плюсы:**
- Активно развивается
- Отзывчивое сообщество
- Тестируемость core-логики
- Кастомный typegen
- Поддерживает web (wasm-bindgen)

</v-click>

</div>
<div>

<v-click>

**Минусы:**
- Pre-1.0, API может меняться
- Бойлерплейт в инфраструктурном/bridge коде
- Мало примеров real-world приложений*

</v-click>

</div>
</div>
