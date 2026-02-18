---
layout: section
---

# Введение в Rust
## Ключевые концепции для мобильных разработчиков

---

# Синтаксис Rust — знакомые концепции

Если вы знаете Swift или Kotlin — вы уже знаете часть Rust

<div class="grid grid-cols-2 gap-4">
<div>

```rust
// Переменные и типы
let name: &str = "Rust";
let mut count: i32 = 0;

// Enum с данными (как в Swift)
enum NetworkResult {
    Success(String),
    Error { code: i32, message: String },
}

// Pattern matching
match result {
    NetworkResult::Success(data) => {
        println!("Данные: {}", data);
    }
    NetworkResult::Error { code, .. } => {
        println!("Ошибка: {}", code);
    }
}
```

</div>
<div>

```swift
// Swift для сравнения
let name: String = "Swift"
var count: Int = 0

enum NetworkResult {
    case success(String)
    case error(code: Int, message: String)
}

switch result {
case .success(let data):
    print("Данные: \(data)")
case .error(let code, _):
    print("Ошибка: \(code)")
}
```

</div>
</div>

---

# Traits и Generics

<div class="grid grid-cols-2 gap-4">
<div>

```rust
// Trait ≈ Protocol (Swift) / Interface (Kotlin)
trait ApiService {
    fn fetch(&self, url: &str) -> Result<String, Error>;
}

struct HttpClient;

impl ApiService for HttpClient {
    fn fetch(&self, url: &str) -> Result<String, Error> {
        Ok("response".to_string())
    }
}

fn load<T: ApiService>(service: &T) {
    let result = service.fetch("/api/data");
}
```

</div>
<div>

```kotlin
// Kotlin для сравнения
interface ApiService {
    fun fetch(url: String): Result<String>
}

class HttpClient : ApiService {
    override fun fetch(url: String): Result<String> {
        // реализация
        return Result.success("response")
    }
}

// Generics с ограничениями
fun <T : ApiService> load(service: T) {
    val result = service.fetch("/api/data")
}
```

</div>
</div>

---

# Ownership — уникальная черта Rust

У каждого значения ровно один владелец.  

```rust {all|1-2|4-5|7-8|all}
let data = vec![1, 2, 3];   // `data` владеет вектором
let data2 = data;           // владение перешло к `data2`

println!("{:?}", data);     // ❌ ОШИБКА КОМПИЛЯЦИИ!
                             // `data` больше не владеет данными

println!("{:?}", data2);    // ✅ ОК — `data2` владелец
```

Компилятор точно знает, когда нужно освободить память — без GC и без ручного управления памятью

---

# Borrow Checker — правила заимствования

Можно не передавать владение значением, а **заимствовать** (`&`)

```rust {all|1-4|6-9|11-14|all}
fn print_len(data: &Vec<i32>) {         // заимствование 
    println!("Длина: {}", data.len());
}                                       // заимствование заканчивается

fn add_element(data: &mut Vec<i32>) {   // мутабельное заимствование
    data.push(42);
}

// Правило: сколько угодно &, ИЛИ одна &mut
let mut items = vec![1, 2, 3];
print_len(&items);           // ✅ иммутабельное заимствование
add_element(&mut items);     // ✅ мутабельное заимствование
print_len(&items);           // ✅ снова можно читать
```

<div class="mt-2 p-3  ">

Borrow checker **гарантирует** отсутствие data races на этапе компиляции.
</div>

---

# Rust vs Swift/Kotlin — безопасность на этапе компиляции

<div class="grid grid-cols-3 gap-4">
<div>

**Проблема**
- Null pointer
- Data races
- Use after free
- Buffer overflow
- Memory Leak

</div>
<div>

**Swift / Kotlin**
- Optional / Nullable ✅
- Runtime проверки ⚠️
- ARC / GC ✅
- Runtime проверки ⚠️
- ARC / GC ✅ (weak ref)

</div>
<div>

**Rust**
- Option\<T\> ✅
- Borrow checker ✅
- Ownership ✅
- Компилятор ✅
- Ownership ✅

</div>
</div>
