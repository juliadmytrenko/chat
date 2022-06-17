<template>
  <div :class="{ chatBubble: true, 'bot': isBot }">
    <div class="date">
      <small>{{ formattedDate }}</small>
    </div>
    <b-card>
      <div class="d-flex">
        <p class="text mb-0">
          {{ text }}
        </p>
      </div>
    </b-card>
  </div>
</template>
<script>
export default {
  props: {
    text: String,
    date: Date,
    isBot: Boolean,
  },
  computed: {
    formattedDate: function () {
      const options = {
        weekday: 'long',
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit',
      }

      return this.date.toLocaleDateString('en', options)
    },
  },
}
</script>
<style lang="scss" scoped>
.date {
  text-align: left;
}

.text {
  text-align: left;
}

.chatBubble {
  margin-left: 4rem;
  margin-right: 0.5rem;
  margin-bottom: 1rem;
}

.bot {
  margin-left: 0.5rem;
  margin-right: 4rem;
}

</style>