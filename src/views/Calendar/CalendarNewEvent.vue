<template>
    <div class="calendar__event">
        <h2>Add new Event: {{activeDay.longName}}</h2>
        <form>
            <label for="event">
                <p>Enter a new event</p>
                <input id="event" v-model="newEvent.title"/>
                <p v-if="errors.exists && errors.title && newEvent.title.length === 0" class="errors">Event must not be empty</p>
            </label>
            <div class="calendar__event--options">
                <label for="work">
                    <p>Work</p>
                    <input type="radio" id="work" v-model="newEvent.eventType" value="work"/>
                </label>
                <label for="personal">
                    <p>Personal</p>
                    <input type="radio" id="personal" v-model="newEvent.eventType" value="personal"/>
                </label>
                <label for="social">
                    <p>Social</p>
                    <input type="radio" id="social" v-model="newEvent.eventType" value="social"/>
                </label>
            </div>
            <p v-if="errors.exists && errors.eventType && newEvent.eventType.length === 0" class="errors">Please select a type</p>
            <button @click.prevent="addNewEvent" >Add Event</button>
        </form>
    </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
export default {
  data () {
    return {
      newEvent: {
        title: '',
        eventType: ''
      },
      activeDay: '',
      errors: {
        exists: false, 
        title: '',
        eventType: ''
      }
    }
  },
  computed: {
    ...mapGetters([
      'getActiveDay'
    ])
  },
  methods: {
    ...mapActions([
      'addEventToDay'
    ]),
    addNewEvent () {
      console.log('Done')
      if(this.newEvent.title.length > 0 && this.newEvent.eventType.length > 0){
        const payload = {
          details: this.newEvent.title,
          type: this.newEvent.eventType,
          edit: false,
          day: this.activeDay.name
        }
        this.$store.dispatch('addEvent', payload)
        this.newEvent = {
          title: '',
          eventType: ''
        } 
        return;
      } else {
        this.errors.exists = true
        if(this.newEvent.title.length === 0){
          this.errors.title = true
        } 
        if(this.newEvent.eventType.length === 0){
          this.errors.eventType = true
        }
      }
    }
  },
  mounted () {
    this.activeDay = this.getActiveDay
  },
  watch: {
    getActiveDay (newValue, oldValue) {
      console.log(`Updating from ${oldValue} to ${newValue}`)
      this.activeDay = newValue
    }
  }
}
</script>

<style>
.errors {
  color: red;
  font-size: 14px;
  margin-top: 7px;
}
</style>
