

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
fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[uniffi::export]
fn hello(name: String) -> String {
    format!("Привет, {}!", name)
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
public func add(a: Int32, b: Int32) -> Int32 { ... }
public func hello(name: String) -> String { ... }
```

Вызов в Swift:

```swift
import MyRustLib

let result = add(a: 2, b: 3)      // 5
let greeting = hello(name: "World")  // "Hello, world!"
```

Типы конвертируются автоматически: `String` ↔ `String`, `Vec<T>` ↔ `[T]`

---

# UniFFI — сгенерированный Kotlin

```kotlin
// uniffi-bindgen generated
fun add(a: Int, b: Int): Int { ... }
fun hello(name: String): String { ... }
```

Вызов в Kotlin:

```kotlin
import com.example.mylib.*

val result = add(2, 3)          // 5
val greeting = hello("World")     // "Hello, world!"
```

Работает через JNA (Java Native Access), без ручного JNI

---

# UniFFI — объекты в Swift / Kotlin

<div class="grid grid-cols-2 gap-4">
<div>

**Swift:**
```swift
let counter = Counter(initial: 0)
counter.increment() // 1
counter.increment() // 2
print(counter.getValue()) // 2
```

</div>
<div>

**Kotlin:**
```kotlin
val counter = Counter(0)
counter.increment() // 1
counter.increment() // 2
println(counter.getValue()) // 2

// Важно! Освободить ресурсы
counter.destroy()
```

</div>
</div>

<div class="mt-4 text-sm">

- Объект живёт на стороне Rust, управляется через `Arc`
- В Kotlin нужно вызывать `.destroy()` (или `.use {}`) — нет автоматической интеграции с GC
- В Swift работает через ARC автоматически

</div>

---

# UniFFI — async функции

```rust
#[uniffi::export]
async fn fetch_data(url: String) -> Result<String, AppError> { ... }
```

<div class="grid grid-cols-2 gap-4 mt-4">
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
</div>

`async fn` → Swift `async/await`, Kotlin `suspend fun`

---

# UniFFI — callback-интерфейсы

Вызов платформенного кода из Rust:

<v-clicks>

```rust
#[uniffi::export(callback_interface)]
pub trait Logger {
    fn log(&self, level: String, message: String);
}

#[uniffi::export]
fn do_work(logger: Box<dyn Logger>) { ... }
```

```swift
// Swift
class SwiftLogger: Logger {
    func log(level: String, message: String) {
        print("[\(level)] \(message)")
    }
}
doWork(logger: SwiftLogger())
```

</v-clicks>

---

# UniFFI — custom types

Маппинг кастомных типов на нативные типы платформ:

```rust {all|1|2|3-6|7|all}
uniffi::custom_type!(UtcDateTime, i64, {
    remote,
    try_lift: |val| {
        DateTime::<Utc>::from_timestamp_millis(val)
            .ok_or(anyhow!("Timestamp {val} out of range"))
    },
    lower: |obj| obj.timestamp_millis(),
});
```

```toml {all|1-5|7-11|all}
# Swift: UtcDateTime → Date
[bindings.swift.custom_types.UtcDateTime]
type_name = "Date"
into_custom = "Date(timeIntervalSince1970: Double({}) / 1000.0)"
from_custom = "Int64({}.timeIntervalSince1970 * 1000)"

# Kotlin: UtcDateTime → java.util.Date
[bindings.kotlin.custom_types.UtcDateTime]
type_name = "java.util.Date"
into_custom = "java.util.Date({})"
from_custom = "{}.time"
```

---

# UniFFI — выводы

<div class="grid grid-cols-2 gap-8">
<div>

**Плюсы:**

<v-clicks>

- Простая интеграция — минимум boilerplate
- Поддержка async/await из коробки
- Callback-интерфейсы для вызова платформенного кода
- Поддержка объектов, enum, ошибок

</v-clicks>

</div>
<div>

**Минусы:**

<v-clicks>

- Нет generics — нужно конкретизировать типы
- Нет интеграции с GC JVM — нужен `.destroy()` / `.use {}`
- Overhead от сериализации через FFI-границу
- Ручная поддержка для сложных типов данных

</v-clicks>

</div>
</div>
