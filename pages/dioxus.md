---
layout: section
---

# Кроссплатформенный UI на Rust

---

# А что, если хочется и UI на Rust?

Подходы Crux и UniFFI: **логика** на Rust, **UI** — нативный (SwiftUI, Compose, React)

<v-click>

Но что если хочется **всё** на Rust — включая UI?

</v-click>

<v-clicks>

- Полностью единая кодовая база — один `main.rs` на все платформы
- Не нужны Swift/Kotlin-разработчики для UI
- Web, Desktop, Mobile — из одного проекта

</v-clicks>

<div v-click class="mt-4 text-center text-lg font-bold">

Несколько фреймворков уже позволяют это делать

</div>

---

# Dioxus

<div class="grid grid-cols-[1fr_auto] gap-8 items-center">
<div>

- **React-like** фреймворк на Rust
- Один `main.rs` → Web, Desktop, iOS, Android
- RSX-макрос (аналог JSX)
- Signals для реактивного состояния
- CLI `dx` — serve, bundle, hot-reload

</div>
<div>

<img src="/images/slide27_img22.png" class="h-50" />

</div>
</div>

<img src="/images/slide28_img23.png" class="h-40 mx-auto mt-4" />

---

# Dioxus — Пример

```rust {all|3-5|7-15|all}
use dioxus::prelude::*;

fn main() {
    dioxus::launch(App);
}

#[component]
fn App() -> Element {
    let mut count = use_signal(|| 0);
    rsx! {
        div { "Счётчик: {count}" }
        button { onclick: move |_| count += 1, "+" }
        button { onclick: move |_| count -= 1, "−" }
    }
}
```

<div class="mt-2 text-center">

Этот код  запускается на Web, Desktop, iOS и Android

</div>

---

# Dioxus — архитектура

<div class="grid grid-cols-2 gap-8">
<div>

**Компонентная модель:**
- Компоненты — обычные Rust-функции
- `#[component]` + `rsx!{}` макрос
- Параметры функции = пропсы

**Состояние (Signals):**
- `use_signal(|| value)` — реактивный сигнал
- Fine-grained: перерисовывается **только** читающий компонент
- Автоматический tracking зависимостей

</div>
<div>

**Рендеринг:**

```
┌─────────────┐
│  Компоненты │  Rust-функции
│  + RSX      │  возвращают Element
└──────┬──────┘
       ▼
┌─────────────┐
│ Virtual DOM │  Diff-движок
└──────┬──────┘
       ▼
┌─────────────┐
│  Renderer   │  Подключаемый
│  (pluggable)│
└─────────────┘
  WebView │ WGPU │ WASM │ SSR
```

</div>
</div>

---

# Dioxus — мобилки

<div class="grid grid-cols-2 gap-8">
<div>

**Текущий подход (0.6):**
- На мобилках рендерится в **WebView**
- iOS — WKWebView, Android — Android WebView
- VirtualDom → HTML/CSS через JS-бридж
- Не нативные виджеты, но:
  - Accessibility бесплатно
  - Маленький бинарь (~5 MB)
  - Используется системный WebView (не Chromium)

</div>
<div>

**Tooling:**

```bash
# Создать проект
dx new

# Запуск на iOS-симуляторе
dx serve --platform ios

# Запуск на Android
dx serve --platform android

# Продакшн-сборка
dx bundle
```

Zero-config — не нужны Gradle, Cocoapods, cargo-mobile2

</div>
</div>

---

# Dioxus — 0.7 и Dioxus Native

<v-clicks>

- **Dioxus 0.7** представляет **Dioxus Native** — новый GPU-рендерер
- Основан на **WGPU + Blitz** (модульный HTML/CSS рендерер)
- Создан совместно с Firefox, Google, Servo, Bevy
- Рендерит HTML/CSS **напрямую на GPU** — без WebView
- Бинарь macOS-приложения < 6 MB
- Поддерживает hot-reload, accessibility, мобильные платформы

</v-clicks>

<div v-click class="mt-4">

| Версия | Рендерер | Технология |
|---|---|---|
| 0.5 / 0.6 | WebView | WKWebView / Android WebView |
| 0.7+ | Dioxus Native (opt-in) | WGPU / Blitz (GPU, без WebView) |

</div>

---

# Slint

<div class="grid grid-cols-2 gap-8">
<div>

- Декларативный GUI-тулкит для **Rust, C++, JS, Python**
- Свой DSL (`.slint` файлы) → компилируется в нативный код
- **Кастомный рендерер** — не WebView:
  - FemtoVG (OpenGL ES 2.0)
  - Skia (Metal, Vulkan, OpenGL)
  - Software renderer (CPU, для embedded)

**Мобильная поддержка:**
- Android — **стабильно** с версии 1.3 (2023)
- iOS — tech preview с версии 1.12
- "Единственный Rust GUI-тулкит с официальной поддержкой Android"

</div>
<div>

```slint
// counter.slint
export component Counter {
    in-out property <int> count: 0;

    VerticalLayout {
        Text { text: "Count: \{count}"; }
        Button {
            text: "+";
            clicked => { count += 1; }
        }
    }
}
```

<div class="mt-2 text-sm">

Версия 1.15 — поддержка safe-area insets и виртуальной клавиатуры на iOS/Android

</div>

</div>
</div>

---

# Makepad и Robius

<div class="grid grid-cols-2 gap-8">
<div>

### Makepad
- **100% GPU-рендеринг** — Metal, DirectX, OpenGL, WebGL
- Live-design DSL с hot-reload
- iOS и Android — через `cargo-makepad`
- Makepad 1.0 вышел в 2025

</div>
<div>

### Robius
- **Мета-проект** для мобильной разработки на Rust
- Makepad (UI) + **Osiris** (платформенные API)
- Osiris — абстракции над: камера, GPS, уведомления, хранилище, сеть
- Цель: не писать платформенный код
- **Robrix** — Matrix-чат клиент на чистом Rust (Makepad + Robius)

</div>
</div>

---

# Сравнение фреймворков

| | **Dioxus** | **Slint** | **Makepad** |
|---|---|---|---|
| Парадигма | React-like, RSX, signals | Декларативный DSL | Hybrid retained/immediate |
| Рендеринг | WebView (0.6) / WGPU (0.7) | Custom (Skia/FemtoVG/CPU) | 100% GPU |
| Hot reload | Да | Да | Да |
| Accessibility | Через WebView | Да (Narrator) | Нет |
| Зрелость | Pre-1.0, быстро развивается | 1.15, стабильный | 1.0 |

---

# Rust GUI для мобилок — текущий стейт

<v-clicks>

- **Ни один фреймворк** пока не достиг зрелости Flutter / React Native
- Dioxus — самый удобный developer experience 
- Robius — амбициозная попытка решить проблему платформенных API

</v-clicks>

<div v-click class="mt-4">

**Когда это имеет смысл:**
- Команда знает Rust, но не знает Swift/Kotlin
- Нужен Web + Mobile + Desktop из одного проекта

</div>
