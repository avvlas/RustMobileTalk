---
layout: section
---

# FFI

---

# Что такое FFI?

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- **Foreign Function Interface** — механизм вызова функций между языками
- Rust компилируется в **нативный код** — можно вызывать через C ABI
- Два направления:
  - Rust → вызывает C/C++ код
  - **C/Swift/Kotlin → вызывают Rust код**

</div>
<div>

```
┌─────────────┐     C ABI     ┌─────────────┐
│   Kotlin    │ ◄──────────► │    Rust     │
│   (JNI)     │               │  (.so/.a)   │
└─────────────┘               └─────────────┘

┌─────────────┐     C ABI     ┌─────────────┐
│    Swift    │ ◄──────────► │    Rust     │
│  (C interop)│               │  (.a)       │
└─────────────┘               └─────────────┘
```

</div>
</div>

---

# Ручной FFI в Rust

```rust
#[no_mangle]
pub extern "C" fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

- `#[no_mangle]` — сохраняет имя функции
- `extern "C"` — используем C ABI (calling convention)

---

# Вызов из Swift (iOS)

<div class="grid grid-cols-2 gap-4">
<div>

**1. Bridging Header**

```c
// RustLib.h
int32_t add(int32_t a, int32_t b);
```

</div>
<div v-click="1">

**2. Вызов в Swift**

```swift
// ViewController.swift
import Foundation

let result = add(2, 3)
print("2 + 3 = \(result)") // 5
```

</div>
</div>

<div v-click="2" class="mt-4">

**Сборка:**
- `cargo build --target aarch64-apple-ios` → `libmylib.a`
- Подключить `libmylib.a` + `RustLib.h` в Xcode

</div>

---

# Вызов из Kotlin (Android)

<div class="grid grid-cols-2 gap-4">
<div>

**1. JNI обёртка в Rust**

```rust
use jni::JNIEnv;
use jni::objects::JClass;

#[no_mangle]
pub extern "system" fn
  Java_com_example_RustLib_add(
    _env: JNIEnv,
    _class: JClass,
    a: i32,
    b: i32,
) -> i32 {
    a + b
}
```

</div>
<div v-click="1">

**2. Вызов в Kotlin**

```kotlin
// RustLib.kt
class RustLib {
    companion object {
        init {
            System.loadLibrary("mylib")
        }

        external fun add(a: Int, b: Int): Int
    }
}

// Использование
val result = RustLib.add(2, 3)
println("2 + 3 = $result") // 5
```

</div>
</div>

<div v-click="2">

**Сборка:**
- `cargo build --target aarch64-linux-android` → `libmylib.so`
- Положить `.so` в `jniLibs/arm64-v8a/`

</div>

---

# Проблемы ручного FFI

- C ABI не поддерживает сложные типы (String, Vec, enum)
- Boilerplate — JNI-обёртки, хедеры .h
- Ручное управление памятью через границу
- Легко допустить ошибку (утечки памяти, UB)

<div v-click="1" class="mt-4 text-center text-xl">

**Решение → UniFFI**
</div>
