<template>
  <div>
    <h1>Create Event,  </h1>
    <h2>{{ user.name }}</h2>
    <h3 >{{ catLength }}</h3>
    <h4>{{ getEvent(1) }}</h4>
    <h5>{{ getEventById(1) }}</h5>

    <form @submit.prevent="createEvent">
      <label for="">Select a category</label>
      <select v-model="event.category">
        <option v-for="cat in categories" :key="cat">{{ cat }}</option>
      </select>

      <h3></h3>
      <div class="field">
        <label for="">Title</label>
        <input v-model="event.title" type="text" placeholder="Add an event title">
      </div>

      <div class="field">
        <label for="">Description</label>
        <input v-model="event.description" type="text" placeholder="Add a description">
      </div>

      <h3>Where is your event?</h3>
      <label for="">Date</label>
      <datepicker v-model="event.date" aria-placeholder="Select a date" />

      <div class="field">
        <label for="">Select a time</label>
        <select v-model="event.time" name="" id="">
          <option v-for="time in times" :key="time" >{{ time }}</option>
        </select>
      </div>

      <input type="submit" class="button -fill-gradient" value="submit">
    </form>
  </div>
</template>

<script>
import { mapState, mapGetters } from 'vuex'
import Datepicker from 'vuejs-datepicker'  

export default {
  components: {
    Datepicker
  },
  data() {
    const times = []
    for (let i = 1; i <= 24; i++) {
      times.push(i + ':00')
    }
    return {
      times,
      categories: this.$store.state.categories,
      event: this.createFreshEventObject()
    }
  },
  methods: {
    createEvent() {
      this.$store.dispatch('createEvent', this.event).then(() => {
        this.$router.push({
          name: 'event-show',
          params: { id: this.event.id}
        })
        this.event = this.createFreshEventObject()
      }).catch(() => {
        console.log('problem creating event')
      })
    },
    createFreshEventObject() {
      const user = this.$store.state.user
      const id = Math.floor(Math.random() * 10000000)
      return {
        id: id,
        category: '',
        organizer: user,
        title: '',
        description: '',
        location: '',
        date: '',
        time: '',
        attendees: []
      }
    }
  },
  computed: {
    catLength() {
      return this.$store.getters.catLength
    },
    getEvent() {
      return this.$store.getters.getEventById
    },
    ...mapState(['user']),
    ...mapGetters(['getEventById'])
  }
}
</script>

<style scoped>
.field {
  margin-bottom: 24px
}
</style>

