<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <div class="bg-red-500 w-full h-10"> Hello </div>
    <input v-model="vdate" type="text" @focus="showPicker = true" @blur="closePicker" />
    <DatePicker v-model="vdate" v-show="showPicker" id="Obi"/>  <div>{{vdate}} {{date}}</div>
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import DatePicker from './components/DatePicker.vue'

function isValidDate(dateObject){
    return new Date(dateObject).toString() !== 'Invalid Date';
}

export default {
  name: 'App',
  data() { return { date: new Date('01-01-2000'), showPicker: false }},
  computed: {
    vdate: {
      set(v) {
        this.date = isValidDate(v) && new Date(v)
      },
      get() {
        return this.date
      }
    }
  },
  components: {
    // HelloWorld,
    DatePicker
  },
  methods: {
    closePicker() {
      setTimeout(() => this.showPicker = false, 500)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  background: lightblue;
}
</style>
