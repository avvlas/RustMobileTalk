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

<div class="flex justify-center items-center h-4/5">
<img src="/images/slide4_img2.png" class="max-h-80 max-w-full object-contain" />
</div>

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
