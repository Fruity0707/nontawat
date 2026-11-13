<script setup lang="ts">
import { ref } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import TestPage from './components/TestPage.vue'
import Introduce from './components/Introduce.vue'
import Calculator from './components/Calculator.vue'

const currentPage = ref<'home' | 'test' | 'introduce' | 'calculator'>('home')
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
      <nav class="menu">
        <button @click="currentPage = 'home'">หน้าหลัก</button>
        <button @click="currentPage = 'test'">หน้าทดสอบใหม่</button>
        <button @click="currentPage = 'introduce'">แนะนำตัว</button>
        <button @click="currentPage = 'calculator'">เครื่องคิดเลข</button>
      </nav>
    </div>
  </header>

  <main>
    <TheWelcome v-if="currentPage === 'home'" />
    <TestPage v-else-if="currentPage === 'test'" />
    <Introduce v-else-if="currentPage === 'introduce'" />
    <Calculator v-else-if="currentPage === 'calculator'" />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}

.menu {
  margin-top: 1rem;
  display: flex;
  gap: 8px;
}

.menu button {
  padding: 6px 14px;
  cursor: pointer;
}

</style>
