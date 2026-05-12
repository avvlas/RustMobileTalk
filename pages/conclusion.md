---
layout: section
---

# Выводы

---

# Выводы

<v-clicks>

- **Rust — зрелый инструмент** для shared-логики в приложениях (Mozilla, 1Password, Signal, Matrix)
- **FFI** — фундамент интеграции 
- **UniFFI** — генерация биндингов из коробки, убирает бойлерплейт
- **Crux** — архитектура и тулинг поверх UniFFI: shared state, эффекты, тестируемость, Web через WASM
- **Dioxus / Slint / Makepad** — Rust UI пока не зрелый, но быстро развивается

</v-clicks>

---

# Ресурсы для изучения Rust

<div class="resource-click-steps" aria-hidden="true">
  <span v-click="1" />
  <span v-click="2" />
  <span v-click="3" />
  <span v-click="4" />
</div>

<div class="rust-resources-grid">
  <ul class="rust-resources-list">
    <li :class="{ 'is-active': $slidev.nav.clicks === 0, 'is-dimmed': $slidev.nav.clicks !== 0 }">
      <a href="https://doc.rust-lang.org/book/" target="_blank" rel="noopener noreferrer">
        The Rust Programming Language
      </a>
    </li>
    <li :class="{ 'is-active': $slidev.nav.clicks === 1, 'is-dimmed': $slidev.nav.clicks !== 1 }">
      <a href="https://github.com/rust-lang/rustlings" target="_blank" rel="noopener noreferrer">
        Rustlings
      </a>
    </li>
    <li :class="{ 'is-active': $slidev.nav.clicks === 2, 'is-dimmed': $slidev.nav.clicks !== 2 }">
      <a href="https://learn.microsoft.com/en-us/training/paths/rust-first-steps/" target="_blank" rel="noopener noreferrer">
        Microsoft: Rust Training
      </a>
    </li>
    <li :class="{ 'is-active': $slidev.nav.clicks === 3, 'is-dimmed': $slidev.nav.clicks !== 3 }">
      <a href="https://google.github.io/comprehensive-rust/" target="_blank" rel="noopener noreferrer">
        Google: Comprehensive Rust
      </a>
    </li>
    <li :class="{ 'is-active': $slidev.nav.clicks >= 4, 'is-dimmed': $slidev.nav.clicks < 4 }">
      <a href="https://raytracing.github.io/books/RayTracingInOneWeekend.html" target="_blank" rel="noopener noreferrer">
        Ray Tracing in One Weekend?
      </a>
    </li>
  </ul>

  <div class="rust-resource-preview">
    <img v-if="$slidev.nav.clicks === 0" src="/images/TheRustProgrammingLanguage.png" alt="The Rust Programming Language" />
    <img v-if="$slidev.nav.clicks === 1" src="/images/Rustlings.png" alt="Rustlings" />
    <img v-if="$slidev.nav.clicks === 2" src="/images/MicrosoftRustTraining.png" alt="Microsoft Rust Training" />
    <img v-if="$slidev.nav.clicks === 3" src="/images/GoogleComprehensiveRust.png" alt="Google Comprehensive Rust" />
    <img v-if="$slidev.nav.clicks >= 4" src="/images/RayTracing.jpg" alt="Ray tracing in one weekend" />
  </div>
</div>

---
layout: center
class: text-center
---

# Спасибо!

<div class="flex justify-center">
  <img
    src="/images/TelegramQR.png"
    alt="Telegram QR"
    class="h-48"
  />
</div>

Власюк Александр

Mobius 2026
