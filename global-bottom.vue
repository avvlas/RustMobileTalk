<template>
  <footer v-if="$nav.currentSlideNo > 1" class="slide-number">
    {{ $nav.currentSlideNo }} / {{ $nav.total }}
  </footer>
</template>

<style scoped>
.slide-number {
  position: fixed;
  right: 1.5rem;
  bottom: 0.75rem;
  font-size: 0.75rem;
  opacity: 0.5;
  pointer-events: none;
  z-index: 100;
}
</style>
