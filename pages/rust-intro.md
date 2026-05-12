---
layout: section
---

# Зачем мне Rust?

---

# Rust


- **Производительный и безопасный**
- **Современный**. Мультипарадигменный, со строгой типизацией и отличным тулингом.
- **Обеспечивает безопасность памяти на этапе компиляции (без Runtime и GC)**
- **Компилируется в LLVM байткод**
- **"If it compiles, it will work"**

---

# Rust — любимый язык программистов по опросам StackOverflow

<div class="flex justify-center items-center h-4/5">
<img src="/images/slide4_img2.png" class="max-h-80 max-w-full object-contain" />
</div>

---

# Rust и AI

- **Rust продолжает набирать популярность** на фоне пузыря ИИ
- **Хорошо подходит для AI agentic кодинга** за счет своей строгости:
<br></br>
  "На Rust трудно запустить неработающий код"

<div class="flex justify-center items-center h-3/5">
<img src="/images/MicrosoftRustAi.png" class="max-h-36 max-w-full object-contain" />
</div>

---

# #RIIR

<div class="flex justify-center items-center">
<img src="/images/RewriteItInRust.png" class="max-h-100 max-w-full object-contain" />
</div>

---

# Где используется Rust

<WhereRustUsed class="h-4/5" :initialCollapsed="true" />

---

# Мотивация - Rust в мобильных приложениях

- **Переиспользование логики** между платформами 
- **Нативные библиотеки** — криптография, сетевые протоколы, кодеки, парсеры
- **Производительность** — без рантайма и GC
- **Безопасность памяти**

---

# Синтаксис Rust

<div class="grid grid-cols-1 gap-4">
<div>

```rust {all|1-3|5-9|11-19|all}
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
</div>

---

# Traits и Generics

<div class="grid grid-cols-1 gap-4">
<div>

```rust {all|1-4|6-12|14-16|all}
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
</div>

---

# Ownership 

У каждого значения ровно один владелец.  

```rust {all|1-2|4-5|7-8|all}
let data = "hello";         // `data` владеет строкой
let data2 = data;           // владение перешло к `data2`

println!("{:?}", data);     // ❌ ОШИБКА КОМПИЛЯЦИИ!
                             // `data` больше не владеет данными

println!("{:?}", data2);    // ✅ ОК — `data2` владелец
```

<v-click>
Компилятор точно знает, когда нужно освободить память — без GC и ручного управления
</v-click>

---

# Borrow Checker

Можно не передавать владение, а **заимствовать** (`&`)

```rust {all|1-3|5-7|9-14|all}
fn print_len(data: &String) { // заимствование 
    println!("Длина: {}", data.len());
} // заимствование заканчивается

fn add_text(data: &mut String) { // мутабельное заимствование
    data.push_str("!");
}

// Правило: сколько угодно &, ИЛИ одна &mut
let mut text = String::from("Rust");
print_len(&text);            // ✅ иммутабельное заимствование
add_text(&mut text);         // ✅ мутабельное заимствование
print_len(&text);            // ✅ снова можно читать
```

<div class="mt-2 p-3  ">

<v-click>

Borrow checker **гарантирует** отсутствие data races на этапе компиляции.

</v-click>
</div>

---

# Rust vs Swift/Kotlin/C++

| | **Swift** | **Kotlin** | **C++** | **Rust** |
|---|---|---|---|---|
| **Modern Language** | ✅ | ✅ | ⚠️ | ✅ |
| **Tooling**| ✅ SPM/Cocoapods | ✅ Gradle | ⚠️ Wtf? | ✅ Cargo |
| **Mobile** | ✅ Native | ✅ Native | ⚠️ JNI/bridging | ✅ (Uni)FFI|
| **Memory Safety** |  ⚠️ Runtime | ⚠️ Runtime | ⚠️ None | ✅ Compile-Time |
| **Performance** | ⚠️ | ⚠️ | ✅ | ✅ |
