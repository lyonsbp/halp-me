<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="getHelp(true)">I need help</button>
    <button @click="getHelp(false)">Nevermind, I got it</button>
    <img v-if="needsHelp" style="width:480px" src="https://media1.giphy.com/media/phJ6eMRFYI6CQ/giphy.gif?cid=ecf05e476dhtlxuyn1kl5ac9npgtl6ft5whahsrrhmc9sbff&rid=giphy.gif&ct=g"/>
  
  </div>
</template>

<script>
import PieSocket from 'piesocket-js'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      socket: null,
      channel: null,
      needsHelp: false
    }
  },
  methods: {
    getHelp(payload) {
      this.channel.publish('help-me', payload)
    },
    async configureSocket() {
      const channel = await this.socket.subscribe('mitzi-alert-test')
      channel.listen('help-me', (data, meta) => {
      console.log('help-me received', data, meta)
      this.needsHelp = data
    })
    this.channel = channel
    }
  },
  async mounted() {
    this.socket = new PieSocket({
      clusterId: 'free3',
      apiKey: 'Oz5TdZPr8UgIZq9xL6Y37kXd3mWfL6jWRo7L0cN3'
    })
    this.configureSocket()
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img {
  display: block;
}
button {
  padding: .5em;
  margin: .5em;
}
</style>
