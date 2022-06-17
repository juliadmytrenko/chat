<template>
  <div>
    <b-card class="chatbow-window text-center" body-class="chatbot-card-body">
      <div class="chatbow-window-inner bg-light h-100">
        <div class="bubbles">
          <chat-bubble
            v-for="(bubble, index) in bubbles"
            :key="index"
            :is-bot="bubble.isBot"
            :text="bubble.message"
            :date="new Date()"
          />
        </div>

        <b-card class="input-section w-100">
          <div class="d-flex">
            <b-form-input
              v-model="text"
              placeholder="Enter your message"
              v-on:keyup.enter="send()"
            ></b-form-input>
            <b-button variant="primary" class="ml-2" @click="send()">
              <BIconArrowReturnLeft aria-label="Send" />
            </b-button>
          </div>
        </b-card>
      </div>
    </b-card>
  </div>
</template>
<script>
import { BIconArrowReturnLeft } from 'bootstrap-vue'
export default {
  components: {
    BIconArrowReturnLeft,
  },
  data() {
    return {
      text: '',
      bubbles: [],
    }
  },
  methods: {
    send() {
      if (this.text === '') {
        return
      }
      this.bubbles.unshift({isBot: false, message: this.text})

      fetch(process.env.flaskURL + '/predict', {
        method: 'POST',
        body: JSON.stringify({ message: this.text }),
        mode: 'cors',
        headers: {
          'Content-Type': 'application/json',
        },
      })
        .then((response) => response.json())
        .then((response) => {
          const message = { isBot: true, message: response.answer }
          this.bubbles.unshift(message)
          this.text = ''
        })
        .catch((error) => {
          console.error('Error:', error)
          const message = { isBot: true, message: "An error has occured. I'm sorry. The server might not be running." }
          this.bubbles.unshift(message)
          this.text = ''
        })
    },
  },
}
</script>
<style lang="scss" scoped>
.chatbow-window {
  max-height: 650px;
  overflow: hidden;
  width: 500px;
  position: fixed;
  top: 10%;
  left: calc(50% - 250px);
}

.chatbow-window-inner {
  position: relative;
}

.input-section {
  position: absolute;
  bottom: 0;
  color: black;
}

.bubbles {
  height: 100%;
  overflow-y: scroll;
  display: flex;
  flex-direction: column-reverse;
  padding-bottom: 75px;
}

.bubble {
  margin: 1rem 1rem 0 1rem;
  color: black;
}

.chatbot-card-body {
  height: 450px;
}
</style>