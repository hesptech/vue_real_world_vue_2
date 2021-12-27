<template>
  <div>
    <h1>Create Event,  </h1>
    <h2>{{ user.name }}</h2>
    <ul>
      <li v-for="cat in categories" :key="cat">{{ cat }}</li>
    </ul>
    <h3 >{{ catLength }}</h3>
    <h4>{{ getEvent(1) }}</h4>
    <h5>{{ getEventById(1) }}</h5>
  </div>
</template>

<script>
import { mapState, mapGetters } from 'vuex'

export default {
  computed: {
    catLength() {
      return this.$store.getters.catLength
    },
    getEvent() {
      return this.$store.getters.getEventById
    },
    ...mapState(['user', 'categories']),
    ...mapGetters(['getEventById'])
  }
}
</script>

<style scoped>

</style>

