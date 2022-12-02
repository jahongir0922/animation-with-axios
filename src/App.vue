<script setup>
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <div id="app">
    <nav>
      <RouterLink :to="{ name: 'home' }">Home</RouterLink>
      <RouterLink :to="{ name: 'about' }">About</RouterLink>
    </nav>
    <RouterView v-slot="{ Component }">
      <transition name="fade" mode="out-in">
        <component :is="Component" />
      </transition>
    </RouterView>
  </div>
</template>

<style scoped lang="scss">
#app {
  display: flex;
  position: relative;
  flex-direction: column;
  nav {
    display: flex;
    justify-content: flex-end;
    gap: 20px;
    padding: 50px;
    a {
      font-size: 25px;
      color: black;
      font-weight: 600;
    }
  }
}
.fade-leave-from {
  left: 0;
  opacity: 1;
}
.fade-leave-to {
  left: -50%;
  opacity: 0;
}
.fade-leave-active {
  transition-timing-function: ease-out;
  transition: 0.25s;
}
.fade-enter-from {
  left: 50%;
  opacity: 0;
}
.fade-enter-to {
  left: 0;
  opacity: 1;
}
.fade-enter-active {
  transition-timing-function: ease-out;
  transition: 0.25s;
}
</style>
