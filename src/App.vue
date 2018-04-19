<template>
  <div id="app">
    <h1>
    </h1>
    <form id="strain" @submit.prevent="searchStrain">
      <div v-for="strain in strains">
        <input type="radio" name="strain" :id="strain" :value="strain" v-model="strainChecked">{{ strain }}<br>
      </div>
      <button type="submit">Search</button>
    </form>
    <!-- <form id="strain-effects">
      <div v-for="effect in effects">
        <input type="radio" name="effect" :id="effect" :value="effect">{{ effect }}<br>
      </div>
    </form> -->

    <ul id="strainOutput">
      <li v-for="strain in strainResponse" :key="strain.id">
        {{ strain.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import { strainApiKey } from './../.env'
import axios from 'Axios'

export default {
  name: 'app',
  data () {
    return {
      strainChecked: '',
      strainResponse: [],
      strainApiKey: strainApiKey,
      msg: 'Let\'s refactor this!',
      strains: ['indica', 'sativa', 'hybrid'],
      effects: ['anxious', 'aroused', 'cramps', 'creative']
    }
  },
  methods: {
    searchStrain: function() {
      // alert(this.strainChecked)
      axios.get(`http://strainapi.evanbusse.com/${strainApiKey}/strains/search/race/${this.strainChecked}`).then(response => {
        this.strainResponse = response.data
      })


    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

// ul {
//   list-style-type: none;
//   padding: 0;
// }
//
// li {
//   display: inline-block;
//   margin: 0 10px;
// }

a {
  color: #42b983;
}
</style>
