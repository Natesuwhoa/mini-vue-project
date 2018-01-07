<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <router-link to="/quote">Get a Quote</router-link>
  </div>
</template>

<script>
  export default {
    name: 'Hello',
    data () {
      return {
        msg: 'Welcome to the quote generator.'
      }
    }
  }
</script>