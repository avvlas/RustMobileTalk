# Crux

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- Кроссплатформа на Rust
- Rust Core, нативный UI

</div>
<div>

<img src="/images/slide21_img16.png" class="h-60 mx-auto" />

</div>
</div>

---

# Crux. Архитектура

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- Архитектура вдохновлена TEA
- Shell - отрисовывает UI, обрабатывает эффекты. Максимально легкий слой
- Core - описывает логику

</div>
<div>

<img src="/images/slide22_img17.png" class="h-60 mx-auto" />

</div>
</div>

---

# Crux. Биндинги

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- Использует UniFFI для генерации биндингов
- Для генерации типов используется кастомный typegen
- Для передачи объектов Rust↔Shell используется бинарная сериализация

</div>
<div>

<img src="/images/slide23_img18.png" class="h-60 mx-auto" />

</div>
</div>

---

# Crux. Kotlin

<div class="grid grid-cols-2 gap-8 items-center">
<div>

- В текущем typegen поддержан только Kotlin/Jvm
- Мой PR с поддержкой Kotlin/Native

</div>
<div>

<img src="/images/slide24_img19.png" class="h-60 mx-auto" />

</div>
</div>

---

# Crux. Пример приложения

https://github.com/redbadger/crux/pull/485

<div class="flex gap-4 justify-center">
<img src="/images/slide25_img20.png" class="h-70" />
<img src="/images/slide25_img21.png" class="h-70" />
</div>

---

# Crux. Текущий стейт

- Активно развивается
- Отзывчивое сообщество
- Мало примеров real world приложений
- Много бойлерплейта в инфраструктурном коде (для typegen, ffi)
