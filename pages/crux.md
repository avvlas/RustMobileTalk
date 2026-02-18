# Crux — зачем, если есть UniFFI?

UniFFI решает только одну задачу — **генерация FFI биндингов**

<v-clicks>

- Нет архитектуры приложения — проектируй сам
- Нет управления состоянием
- Нет системы сайд-эффектов
- Нет поддержки Web (WASM)
- Нет стратегии тестирования shared-логики

</v-clicks>

<div v-click class="mt-4 text-lg font-bold text-center">

UniFFI — это сантехника. Crux — это архитектура, которая эту сантехнику использует.

</div>

---

# Crux — что это?

<div class="grid grid-cols-2 gap-8 items-center h-full">
<div>

- Кроссплатформенный фреймворк от **Red Badger**
- Вся бизнес-логика — в Rust **Core**
- UI — нативный: SwiftUI, Jetpack Compose, React
- Web — first-class через WASM

</div>
<div>

<img src="/images/slide21_img16.png" class="scale-150" />

</div>
</div>

---

# Crux — Core + Shell

<div class="grid grid-cols-2 gap-8 items-center">
<div>

Архитектура вдохновлена **TEA** (The Elm Architecture):

- **Core** (Rust) — чистый, без сайд-эффектов
  - `Model` — состояние
  - `Event` — входные события
  - `update()` — логика переходов
  - `ViewModel` — данные для UI
- **Shell** (Swift/Kotlin/TS) — тонкий слой
  - Рисует UI
  - Выполняет эффекты (HTTP, KV, ...)
  - Передаёт события в Core

</div>
<div>

<img src="/images/slide22_img17.png" class="h-60 mx-auto" />

</div>
</div>

---

# Crux — цикл обработки

```
User interaction
    → Shell создаёт Event
        → Core.process_event(event)
            → update() мутирует Model, возвращает Vec<Request>
                → Shell выполняет Request (HTTP, KV, Time, ...)
                    → Shell вызывает handle_response(uuid, Response)
                        → Core обрабатывает, может вернуть ещё Request'ы
                            → Цикл завершается Render-запросом
                                → Shell читает ViewModel, обновляет UI
```

<div class="mt-4">

Каждый `Request` содержит **UUID** — Shell сопоставляет Response с оригинальным запросом

</div>

---

# Crux — Capabilities (эффекты)

Core **никогда** не выполняет I/O. Он только **описывает намерение** через Capabilities:

| Capability | Крейт | Паттерн |
|---|---|---|
| `Render` | `crux_core` | Fire-and-forget |
| `Http` | `crux_http` | Request → Response |
| `KeyValue` | `crux_kv` | Request → Response |
| `Time` | `crux_time` | Request → Response |
| `Platform` | `crux_platform` | Request → Response |

<v-click>

Можно писать **кастомные** Capabilities — Bluetooth, камера, GPS, ...

Shell реализует выполнение на каждой платформе

</v-click>

---

# Crux — почему Core без сайд-эффектов?

<v-clicks>

- **WASM**: WebAssembly — песочница, прямой I/O невозможен
- **Тестируемость**: Core детерминирован — тесты за **миллисекунды**, без эмуляторов
- **Безопасность**: Core физически не может обратиться к сети/файлам — защита от supply-chain атак

</v-clicks>

<div v-click class="mt-4">

```rust
// Тест — просто "ещё один Shell"
let app = AppTester::<App>::default();
let mut model = Model::default();

let update = app.update(Event::Increment, &mut model);

assert_eq!(model.count, 1);
assert_effect!(update, Effect::Render(_));
```

</div>

---

# Crux — биндинги и typegen

<div class="grid grid-cols-2 gap-8 items-center">
<div>

Двойной подход к FFI:

1. **UniFFI / wasm-pack** — механизм вызова функций через FFI
2. **Кастомный typegen (serde-generate)** — генерация типов данных

<v-click>

Почему не хватает одного UniFFI?
- Сложные Rust `enum` с данными (tagged unions)
- Глубоко вложенные generic-типы
- Нужен единый подход для **Swift + Kotlin + TypeScript**
- UniFFI вообще **не поддерживает Web**

</v-click>

</div>
<div>

<img src="/images/slide23_img18.png" class="h-60 mx-auto" />

</div>
</div>

---

# Crux — typegen в деталях

Типы передаются через **бинарную сериализацию** (serde):

```
Rust Event/ViewModel  →  serde serialize  →  bytes  →  serde deserialize  →  Swift/Kotlin/TS
```

<v-click>

`serde-generate` создаёт **зеркальные типы** на целевом языке:

- Rust `enum` с payload → Swift `enum` с associated values
- Rust `enum` с payload → Kotlin `sealed class`
- Rust `enum` с payload → TypeScript discriminated union

</v-click>

<v-click>

Изменил `Event` в Rust → сгенерированный Swift/Kotlin/TS **не компилируется** → ошибка найдена на этапе сборки

</v-click>

---

# Crux — Kotlin/Native

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- В текущем typegen поддержан только Kotlin/JVM
- `serde-generate` генерирует JVM-специфичный код
- Мой PR с поддержкой **Kotlin/Native** в serde-generate

</div>
<div>

<img src="/images/slide24_img19.png" class="" />

</div>
</div>

---

# Crux — пример приложения

https://github.com/redbadger/crux/pull/485

<div class="flex gap-4 justify-center">
<img src="/images/slide25_img20.png" class="h-70" />
<img src="/images/slide25_img21.png" class="h-70" />
</div>

---

# Crux — текущий стейт

<div class="grid grid-cols-2 gap-8">
<div>

**Плюсы:**
- Активно развивается
- Отзывчивое сообщество
- Web — first-class citizen
- Тесты core-логики за миллисекунды
- Compile-time safety через typegen

</div>
<div>

**Минусы:**
- Pre-1.0, API может меняться
- Мало примеров real-world приложений
- Много бойлерплейта в инфраструктурном коде
- `serde-generate` — не идеальное решение для typegen

</div>
</div>
