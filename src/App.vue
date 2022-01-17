<template lang="pug">
header
main
  .container
    <Beer/>
    <Profile/>
footer
</template>

<script>
import Profile from './components/profile.vue'
import Beer from './components/beer.vue'

export default {
  name: 'App',
  components: {
    Profile,
    Beer
  }
}
</script>
