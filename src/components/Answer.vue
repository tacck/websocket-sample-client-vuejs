<template>
  <div class="container">
    <div class="side-padding">
      <b-alert variant="success" show>{{ selections[0].count }}</b-alert>
    </div>
    <div class="side-padding">
      <b-alert variant="warning" show>{{ selections[1].count }}</b-alert>
    </div>
    <b-button class="side-padding" variant="primary" @click="send(0)">{{ selections[0].name }}</b-button>
    <b-button class="side-padding" variant="warning" @click="send(1)">{{ selections[1].name }}</b-button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ws: null,
      selections: [
        {
          name: '',
          count: 0
        },
        {
          name: '',
          count: 0
        }
      ]
    }
  },
  mounted() {
    this.ws = new WebSocket(process.env.VUE_APP_WS_SERVER)
    const self = this
    this.ws.addEventListener('message', message => {
      const obj = JSON.parse(message.data)
      const type = obj.message
      const data = obj.data
      if (type === 'selections') {
        self.selections = data
      }
    })
  },
  methods: {
    send(id) {
      this.ws.send(JSON.stringify(
        {
          message: 'post',
          data: {
            id: id
          }
        }
      ))
    }
  }
}
</script>

<style>
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
.side-padding {
  margin: 0 20px;
}
</style>