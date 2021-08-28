<script setup>
  import { ref, onMounted } from 'vue'
  import Header from './components/Header.vue'
  import ProfileCard from './components/ProfileCard.vue'

  let profile = ref({})
  let username = ref('octocat')
  let errorMessage = ref('')

  onMounted(() => {
    search()
  })

  function onInput() {
    if (errorMessage.value) {
      errorMessage.value = ''
    }
  }

  function search() {
    fetch(`https://api.github.com/users/${username.value}`)
    .then(async (response) => {
      if (response.ok) {
        const data = await response.json()
        console.log(data)
        profile.value = data
        username.value = ""
      } else {
        errorMessage.value = "No Results"
        console.log(errorMessage.value)
      }
      
    })
  }
</script>

<template>
  <Header />

  <main>

    <div class="search">
      <form @submit.prevent="search">
        
        <img src="./assets/images/icon-search.svg" alt="" />
        
        <input type="text" 
          v-model="username" 
          @input="onInput" 
          placeholder="Search GitHub username…">
        
        <div v-if="errorMessage" class="error_message">{{ errorMessage }}</div>
        
        <button class="btn"> Search </button>

      </form>
    </div>

    <ProfileCard :profile="profile" />
  </main>
</template>

<style>
#app {
  max-width: 375px;
  padding: 32px 24px;
  margin-left: auto;
  margin-right: auto;
}

main > * {
  margin-bottom: 16px;
}

.search {
  background: var(--color-white);
  border-radius: 15px;
  padding: .6em .6em .6em 1.2em;
}
.search form {
  display: flex;
  align-items: center;
}
body.dark-mode .search {
  background: #1E2A47;
}
.search input {
  flex: 1;
  margin: 0 .6em;
  color: #4B6A9B;
  background: transparent;
  border: none;
  outline:none;
  padding: .8em
}
.search input::placeholder,
.search input::-webkit-input-placeholder,
.search input::-moz-placeholder {
  color: #4B6A9B;
}
body.dark-mode .search input,
body.dark-mode .search input::placeholder,
body.dark-mode .search input::-webkit-input-placeholder,
body.dark-mode .search input::-moz-placeholder {
  color: #ffffff;
}
.search .error_message {
  display: none;
}
.search .btn {
  font-size: 1.1em;
  line-height: 1.6em;
  font-weight: 700;
  padding: .9em 1.3em;
  border: none;
  background: #0079FF;
  color: var(--color-white);
  border-radius: 10px;
  cursor: pointer;
}

@media screen and (min-width: 768px) {
  #app {
    max-width: 573px;
    padding: 140px 98px;
  }
  main > * {
    margin-bottom: 24px;
  }
  .search {
    padding: .6em .6em .6em 2.1em;
  }
  .search input,
  .search input::placeholder,
  .search input::-webkit-input-placeholder,
  .search input::-moz-placeholder {
    font-size: 1.2em;
  }
  .search .error_message {
    display: block;
    color: var(--color-error);
    font-weight: 700;
    padding: 0 1.5em;
  }
}

@media screen and (min-width: 1440px) {
  #app {
    max-width: 730px;
  }
  .search .btn:hover {
    background: #60ABFF;
  }
  .search input:hover {
    cursor: pointer;
  }
}
</style>