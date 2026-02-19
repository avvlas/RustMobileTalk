---
layout: section
---

# Кроссплатформенный UI на Rust

---

# А что, если хочется и UI на Rust?

<v-clicks>

- Подходы Crux и UniFFI: **логика** на Rust, **UI** — нативный (SwiftUI, Compose, React)
- Но что если хочется **всё** на Rust — включая UI?

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

<img src="/images/slide27_img22.png" class="h-52" />

</div>
</div>

<img src="/images/slide28_img23.png" class="h-52 mx-auto mt-4" />

---

# Dioxus — Пример

<div class = "grid grid-cols-2 gap-8">
<div>

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

</div>

<div>
<v-click>

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

</v-click>
</div>
</div>

---

# Dioxus - Рендеринг

<div class="grid grid-cols-2 gap-8">
<div>

<v-click>

**Текущий подход (0.6):**
- На мобилках рендерится в **WebView**
- iOS — WKWebView, Android — Android WebView
- VirtualDom → HTML/CSS через JS-бридж

</v-click>

</div>
<div>
<v-click>

**Dioxus Native (0.7):**
- Кастомный рендерер на WGPU (Blitz)
- Нативно, без WebView

</v-click>

</div>
</div>

---

# Slint

<div class="grid grid-cols-2 gap-8">
<div>

<v-clicks>

- Декларативный GUI-тулкит для **Rust, C++, JS, Python**
- Свой DSL (`.slint` файлы) → компилируется в нативный код
- **Кастомный рендерер**:
  - FemtoVG (OpenGL ES 2.0)
  - Skia (Metal, Vulkan, OpenGL)
  - Software renderer (CPU, для embedded)
  
</v-clicks>
</div>
<div>


```rust
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


</div>
</div>

---

# Makepad

<v-clicks>

- **100% GPU-рендеринг** — Metal, DirectX, OpenGL, WebGL
- Live-design DSL с hot-reload
- iOS и Android — через `cargo-makepad`
- Makepad 1.0 вышел в 2025

</v-clicks>

---

# Robius

<v-clicks>

- **Мета-проект** для мобильной разработки на Rust
- Makepad (UI) + **Osiris** (платформенные API)
- Osiris — абстракции над: камера, GPS, уведомления, хранилище, сеть
- Цель: не писать платформенный код
- **Robrix** — Matrix-чат клиент на чистом Rust (Makepad + Robius)

</v-clicks>

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

- Ни один фреймворк пока не достиг зрелости Flutter / React Native
- Dioxus — самый удобный developer experience 
- Robius — амбициозная попытка решить проблему платформенных API

</v-clicks>

<div v-click class="mt-4">

**Когда это имеет смысл:**
- Команда знает Rust, но не знает Swift/Kotlin
- Нужен Web + Mobile + Desktop из одного проекта

</div>
