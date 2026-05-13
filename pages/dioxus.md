---
layout: section
---

# Кроссплатформенный UI на Rust

---

# Кроссплатформенный UI на Rust

- **Dioxus** - Flutter на Rust. Рендерится *в WebView*.
- **Slint** - Поддерживает несколько рендеров, включая *skia*.
- **Makepad** - *GPU-first* рендеринг.

---

# Dioxus

<div class="grid grid-cols-[1fr_auto] gap-8 items-center">
<div>

- **React-like** фреймворк на Rust
- Web, Desktop, iOS, Android
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

```rust
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

<!-----

# Dioxus - Рендеринг

<div class="grid grid-rows-2 gap-8">
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
</div>-->

---

# Slint

<div class="grid grid-cols-2 gap-8">
<div>

- **Декларативный GUI-тулкит** для Rust
- **Поддерживает несколько рендеров**:
  - QT
  - Skia 
  - FemtoVG 
  - Software renderer (CPU, для embedded)
  
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


<div class="grid grid-cols-2">
<div>

- GPU-first рендеринг
- Live-design DSL с hot-reload
- iOS и Android — через `cargo-makepad`
- Makepad 1.0 вышел в 2025

</div>

<div>

```rust
Container = {{View}} {
    
    Button1 = <Button> {
        width: 100,
        label: "Click me"
    }

    Panel = <View> { 
        flow: Down,
        Button2 = <Button> {
            width: 200
        }
    }
}
```

</div>
</div>

---

# Robius

<div class="grid grid-cols-2">
<div>

- **Мета-проект** для мобильной разработки на Rust
- **Цель** — не писать платформенный код
- **Makepad** для UI
- **Osiris** — абстракции над платформенными API (камера, GPS, уведомления, хранилище, сеть)
- **Robrix** — Matrix-клиент на чистом Rust (Makepad + Robius)

</div>

<div>

<img src="/images/Robrix.png" class="scale-45 -mt-65 border border-black" />

</div>
</div>

<!-----

# Сравнение фреймворков

| | **Dioxus** | **Slint** | **Makepad** |
|---|---|---|---|
| **Парадигма** | React-like, RSX, signals | Декларативный DSL | Hybrid retained/immediate |
| **Рендеринг** | WebView (0.6) / WGPU (0.7) | Custom (Skia/FemtoVG/CPU) | 100% GPU |
| **Hot reload** | Да | Да | Да |
| **Accessibility** | Через WebView | Да (Narrator) | Нет |
| **Зрелость** | Pre-1.0, быстро развивается | 1.15, стабильный | 1.0 |-->

---

# Rust GUI для мобилок

<div class="h-80 flex items-center">
<div>

- Ни один фреймворк еще не достиг зрелости Flutter / React Native
- Dioxus — самый удобный developer experience 
- Robius — амбициозная попытка решить проблему платформенных API

</div>
</div>
