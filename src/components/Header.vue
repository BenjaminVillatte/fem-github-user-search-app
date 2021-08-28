<script setup>
  import { ref, onMounted } from 'vue'

  const darkMode = ref(false)

  onMounted(() => {
    darkMode.value = window.matchMedia("(prefers-color-scheme: dark)").matches
    updateBodyClass()
  })

  const switchMode = (e) => {
    e.preventDefault()
    darkMode.value = !darkMode.value
    updateBodyClass()
  }

  const updateBodyClass = () => {
    if (darkMode.value) {
      document.body.classList.add('dark-mode')
    } else {
      document.body.classList.remove('dark-mode')
    }
  }
</script>

<template>
  <header class="header">
    <h1>devfinder</h1>

    <div class="theme-switcher" @click="switchMode">
      <template v-if="darkMode">
        Light 
        <img src="../assets/images/icon-sun.svg" alt="" />
      </template>
      <template v-else>
        Dark 
        <img src="../assets/images/icon-moon.svg" alt="" />
      </template>
    </div>
  </header>
</template>

<style>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 36px;
}
</style>
