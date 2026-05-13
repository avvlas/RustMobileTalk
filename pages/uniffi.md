# UniFFI

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- Проект **Mozilla** 
- Автоматическая генерация FFI биндингов в Rust 

</div>
<div>

<img src="/images/mozilla_uniffi.png" class="scale-120" />

</div>
</div>

---

# Поддерживаемые языки

<div class="grid grid-cols-2 gap-8">
<div>

**Official:**
- Swift
- Kotlin/JVM
- Python
- Ruby

</div>
<div>

**Third Party:**
- C 
- C++
- C#
- Java
- Dart
- Go
- Kotlin Native

</div>
</div>

---

# UniFFI — простой пример

```rust
// lib.rs

#[uniffi::export]
fn hello(name: String) -> String {
    format!("Hello, {}!", name)
}

uniffi::setup_scaffolding!();
```

---

# UniFFI - интеграция в проект 
##  

**iOS (Xcode):**
1. `cargo run --bin uniffi-bindgen generate ...` → `.swift` файл
2. Добавить `.a` + `.swift` в Xcode проект

**Android (Gradle):**
- **Rust Android Gradle Plugin** 
- **Gobley** (для KMP проекта)

---

# UniFFI — сгенерированный Swift

```swift
// uniffi-bindgen generated
public func hello(name: String) -> String { ... }
```

Вызов в Swift:

```swift
import MyRustLib

let greeting = hello(name: "World")  // "Hello, world!"
```

Типы конвертируются автоматически: `String` ↔ `String`, `Vec<T>` ↔ `[T]`

---

# UniFFI — сгенерированный Kotlin

```kotlin
// uniffi-bindgen generated
fun hello(name: String): String { ... }
```

Вызов в Kotlin:

```kotlin
import com.example.mylib.*

val greeting = hello("World")     // "Hello, world!"
```

Работает через JNA (Java Native Access), без ручного JNI

---

# UniFFI — Lifting & Lowering

<UniffiLiftLowerDiagram />

---

# UniFFI — объекты в Swift 

```swift
let counter = Counter(initial: 0)
counter.increment() // 1
counter.increment() // 2
print(counter.getValue()) // 2
```

Объект живёт на стороне Rust, управляется через `Arc`


---

# UniFFI — объекты в Kotlin

```kotlin
val counter = Counter(0)
counter.increment() // 1
counter.increment() // 2
println(counter.getValue()) // 2

// Важно! Освободить ресурсы
counter.destroy()
```

Нет интеграции с GC JVM – В Kotlin нужно вызывать `.destroy()` (или `.use {}`).

---

# UniFFI — custom types

Маппинг кастомных типов на нативные типы платформ:

<!--
```rust {all|1|3-6|7|none}
uniffi::custom_type!(UtcDateTime, i64, {
    remote,
    try_lift: |val| {
        DateTime::<Utc>::from_timestamp_millis(val)
            .ok_or(anyhow!("Timestamp {val} out of range"))
    },
    lower: |obj| obj.timestamp_millis(),
});
```
-->

```toml 
# Rust UtcDateTime → Kotlin java.util.Date
[bindings.kotlin.custom_types.UtcDateTime]
type_name = "java.util.Date"
into_custom = "java.util.Date({})"
from_custom = "{}.time"
```

---

# UniFFI — callback-интерфейсы

Вызов платформенного кода из Rust:

```rust {all|none}
// Rust
#[uniffi::export(callback_interface)]
pub trait Logger {
    fn log(&self, level: String, message: String);
}

#[uniffi::export]
fn do_work(logger: Box<dyn Logger>) { ... }
```

```swift {none|all}
// Swift
class SwiftLogger: Logger {
    func log(level: String, message: String) {
        print("[\(level)] \(mess/Matrixage)")
    }
}
doWork(logger: SwiftLogger())
```

---

# UniFFI — async функции

```rust
#[uniffi::export]
async fn fetch_data(url: String) -> Result<String, AppError> { ... }
```

<div class="grid grid-rows-2 gap-4 mt-4">
<v-clicks>
<div>

**Swift:**

```swift
let data = try await fetchData(url: "https://...")
```

</div>
<div>

**Kotlin:**
```kotlin
val data = fetchData("https://...")  // suspend fun
```

</div>
</v-clicks>
</div>

---

# UniFFI — выводы

<div class="grid grid-cols-2 gap-8">
<div>

<v-click>

**Плюсы:**

- Простая интеграция — минимум boilerplate
- Поддержка async/await из коробки
- Callback-интерфейсы для вызова платформенного кода
- Поддержка объектов, enum, ошибок

</v-click>

</div>
<div>

<v-click>

**Минусы:**

- Нет generics — нужно конкретизировать типы
- Нет интеграции с GC JVM — нужен `.destroy()` / `.use {}`
- Overhead от сериализации через FFI-границу
- Нет поддержки WASM

</v-click>

</div>
</div>
