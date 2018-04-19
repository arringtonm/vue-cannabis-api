<template>
  <div id="app">
    <h1>Grooooovy
    </h1>
    <ul>
      <li id="searchTabsName" class="searchNav" v-bind:class="{ active : searchTab == 'name'}" @click="searchTab = 'name'">Name</li>
      <li id="searchTabsRace" class="searchNav" v-bind:class="{ active : searchTab == 'race'}" @click="searchTab = 'race'">Race</li>
      <li id="searchTabsEffects" class="searchNav" v-bind:class="{ active : searchTab == 'effects'}" @click="searchTab = 'effects'">Effects</li>
    </ul>
    <div id="searchBox">



<!-- name tab -->

      <div v-if="searchTab == 'name'">
        <form id="name" @submit.prevent="searchName">
          <input type="text" placeholder="Strain name or keyword" v-model="searchByName">
          <button type="submit">Search</button>
        </form>


      </div>

<!-- race tab  -->
      <div v-if="searchTab == 'race'">
        <form id="strain" @submit.prevent="searchRace">
          <div v-for="strain in strains">
            <input type="radio" name="strain" :id="strain" :value="strain" v-model="raceChecked">{{ strain }}<br>
          </div>
          <button type="submit">Search</button>
        </form>
      </div>

<!-- effects tab  -->
      <div v-if="searchTab == 'effects'">
        <form id="strain-effects">
          <div v-for="effect in effects">
            <input type="radio" name="effect" :id="effect" :value="effect">{{ effect }}<br>
          </div>
        </form>
      </div>
    </div>


      <ul id="strainOutput">
        <li v-for="strain in strainResponse" :key="strain.id" class="strainname" @click="searchDetails(strain.id)">
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
      searchTab: 'race',
      strainChecked: '',
      searchByName: '',
      strainId: '',
      strainIdResponse: '',
      strainResponse: [],
      strainApiKey: strainApiKey,
      msg: 'Let\'s refactor this!',
      strains: ['Indica', 'Sativa', 'Hybrid'],
      effects: ['anxious', 'aroused', 'cramps', 'creative']
    }
  },
  methods: {
    searchRace: function() {
      axios.get(`http://strainapi.evanbusse.com/${strainApiKey}/strains/search/race/${this.raceChecked}`).then(response => {
        this.strainResponse = response.data
      })
    },
    searchDetails: function(strain) {
      axios.get(`http://strainapi.evanbusse.com/${strainApiKey}/strains/data/desc/${strain}`).then(response => {
        this.strainIdResponse = response.data
      })
      // console.log(strain);
    }
  }
}
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}

body {
  background-color: #F3F3F1;
}

#app {
  padding: 100px;
  font-family: 'Nunito', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  color: #444;
  font-weight: 200;
  // margin-top: 60px;
}

.strainname {
  text-decoration: underline;
  text-decoration-color: green;
  &:hover {
    cursor: pointer
  }
}

#searchBox {
  margin-top: -6px;
  border: 1px solid #83468c;
  background-color: white;
  padding: 25px;
}

li.searchNav {
  // width: 70px;
  display: inline-block;
  // justify-content: center;
 // border: 1px solid #999;
  padding: 10px;
  // border-bottom: 5px solid #F3F3F1;
  // border-bottom: 1px solid #83468c;
  margin-left: -3px;
  margin-right: -2px;
  color: #777;
  margin-bottom: -16px;
  &:hover {
    cursor: pointer;
  }

}

.active {
  // border-bottom: 5px solid #83468c !important;
  border: 1px solid #83468c;

  border-bottom: 1px solid white;
  color: #444 !important;
  background-color: white;

}

h1, h2 {
  font-weight: 900;
}


// purple: #83468c
// yellow: #F7E042
// beige: #F3F3F1

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
