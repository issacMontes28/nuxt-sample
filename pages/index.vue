<template>
  <div>
    <h1>Events</h1>
    <EventCard v-for="(event, index) in events" :key="index" :event="event" :data-index="index" />
  </div>
</template>
<script>
import EventCard from '@/components/EventCard.vue'
//import EventService from '@/services/EventService'
import { mapState } from 'vuex'

export default {
  head() {
    return {
      'title': 'Event Listening'
    }
  },
  //we not longer need asyncData since we're not working with component data, but with vuexStore data
  /*async asyncData({ error }) {
    try {
      const { data } = await EventService.getEvents()

      return {
        events: data
      }

    } catch (e) {
      error({
        statusCode: 503, message: 'Unable to fetch events at this time, please try again'
      })
    }
  },*/
  async fetch({ store, error }) {
    try {
      await store.dispatch('events/fetchEvents')
    } catch (e) {
      error({
        statusCode: 503, message: 'Unable to fetch events at this time, please try again'
      })
    }
  },
  computed: mapState({
    events: state => state.events.events
  }),
  components: {
    EventCard
  }
}
</script>