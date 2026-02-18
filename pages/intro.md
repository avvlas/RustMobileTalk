# План

<div class="grid grid-cols-2 gap-8 items-center">
<div>

1. Введение в Rust
2. Примеры приложений на Rust
3. FFI — вызов Rust из Swift/Kotlin
4. UniFFI — генерация биндингов
5. CRUX — аналог KMP на Rust
6. Кроссплатформенный UI на Rust
7. Заключение. Выводы

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

<div class="flex justify-center items-center h-4/5">
<img src="/images/slide4_img2.png" class="max-h-80 max-w-full object-contain" />
</div>

---

# Где используется Rust

<WhereRustUsed class="h-4/5" :initialCollapsed="true" />

---

# Мотивация - Rust в мобильных приложениях

- **Нативные библиотеки** — криптография, сетевые протоколы, кодеки, парсеры
- **Производительность** — hot path без GC-пауз
- **Безопасность памяти** 
- **Переиспользование логики** между платформами 

---

# Rust vs C++

| | **C/C++** | **Rust** |
|---|---|---|
| Modern Language | ⚠️ | ✅ |
| Tooling | CMake, Makefiles | Cargo ✅ |
| iOS/Android | NDK + bridging | UniFFI ✅ |
| Memory safety | ⚠️ | Компилятор ✅ |
| Thread safety | ⚠️ | Borrow checker ✅ |
