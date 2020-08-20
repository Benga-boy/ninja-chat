<template>
  <div class="new-message">
    <form @submit.prevent="addMessage">
      <label for="new-message">New Message (enter to add):</label>
      <p v-if="feedback" class="red-text">{{feedback}}</p>
      <input type="text" name="new-message" v-model="newMessage">
    </form>
  </div>
</template>

<script>
import db from '@/firebase/init'
export default {
  name: "NewMessage",
  props: ['name'],
  data() {
    return {
      newMessage: null,
      feedback: null
    }
  },
  methods: {
    // ! Send a new message * name is coming passed as a prop from Welcome.vue
    addMessage() {
      if (this.newMessage) {
        db.collection('messages').add({
          content: this.newMessage,
          name: this.name,
          timestamp: Date.now()
        }).catch(err => {
          console.log(err)
        })
        this.newMessage = null
        this.feedback = null
      } else {
        this.feedback = 'Please enter a message'
      }
    }
  }
}
</script>

<style>

</style>