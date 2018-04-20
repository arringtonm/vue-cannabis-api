<template>
  <div id="app">
    <h1>Groooovy</h1>
    <!-- <h2>Weedipedia </h2> -->

    <div id="searchTabAndBox">
    <ul>
      <!-- <li><h2>Search</h2></li> -->

      <li id="searchTabsName" class="searchNav" v-bind:class="{ active : searchTab == 'name'}" @click="searchTab = 'name'">Name</li>
      <li id="searchTabsType" class="searchNav" v-bind:class="{ active : searchTab == 'type'}" @click="searchTab = 'type'">Type</li>
      <li id="searchTabsEffects" class="searchNav" v-bind:class="{ active : searchTab == 'effects'}" @click="searchTab = 'effects'">Effects</li>
    </ul>

    <div id="searchBox">


<!-- name tab -->
      <!-- <transition name="fade"> -->
        <div v-if="searchTab == 'name'" key="searchName">
          <form id="name" @submit.prevent="searchName">
            <input type="text" placeholder="Strain name or keyword" v-model="searchByName">

            <button type="submit">Search</button>
          </form>


        </div>

  <!-- type tab  -->
        <div v-if="searchTab == 'type'" key="searchType">
          <form id="strain" @submit.prevent="searchType">
            <div v-for="strain in strains">
              <input type="radio" name="strain" :id="strain" :value="strain" v-model="typeChecked">{{ strain }}<br>
            </div>
            <button type="submit">Search</button>
          </form>
        </div>

  <!-- effects tab  -->
        <div v-if="searchTab == 'effects'" key="searchEffects">
          <form id="strain-effects">
            <div v-for="effect in effects">
              <input type="radio" name="effect" :id="effect" :value="effect">{{ effect }}<br>
            </div>
          </form>
        </div>
      <!-- </transition> -->
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
      searchTab: 'type',
      strainChecked: '',
      searchByName: '',
      typeChecked: '',
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
    searchType: function() {
      axios.get(`http://strainapi.evanbusse.com/${strainApiKey}/strains/search/race/${this.typeChecked}`).then(response => {
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
  // background: #F3F3F1 url("../img/weed1.jpg") center;
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
  margin-top: 30px;
  border: 1px solid #83468c;
  background-color: white;
  // border-radius: 15px;
  padding: 25px;
}

#searchTabAndBox {
  margin-top: -100px;
}


ul {
  margin-left: -25px;
}

li.searchNav {
  display: inline-block;
  padding: 0px 10px;
  margin-left: -3px;
  margin-right: -2px;
  color: #777;
  font-size: 30px;
  margin-bottom: -16px;
  // transition: color 200s;
  &:hover {
    cursor: pointer;
    color: #83468c !important;
    // border-bottom: 5px solid rgba(131, 70, 140,0.25);
  }

}


.active {
  // border-bottom: 5px solid #83468c !important;
  // border: 1px solid #83468c;
  // border-bottom: 2px solid white !important;
  // border-top-left-radius: 15px;
  border-top-right-radius: 15px;
  border-bottom: 5px solid #83468c;
  color: #83468c !important;
  font-weight: 400;
  // background-color: white;

}

h1 {
  color: #444;
  font-weight: 900;
  font-size: 120px;
  animation-name: flex;
  animation-duration: 700ms;
  // animation-iteration-count: infinite;
  // text-decoration: underline;
  &:hover {

    animation-name: glint;
    animation-duration: 1050ms;
    // animation-iteration-count: infinite;
  }
}

h2 {
  margin-top: -90px;
  // transform: translateX(200px);
  font-weight: 400;
  font-size: 40px;
  color: #83468c;
}

button:active {
    // background-color: #444;
    // animation-name: glint2;
    // animation-duration: 200ms;

}

// purple: #83468c
// yellow: #F7E042
// beige: #F3F3F1
// aqua: #6AD2EF
// orange: #FC8150


// ul {
//   list-style-type: none;
//   padding: 0;
// }
//
// li {
//   display: inline-block;
//   margin: 0 10px;
// }


.fade-enter-active, .fade-leave-active {
  transition: opacity .05s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

@keyframes glint {

  // darker versions of electric lettuce color palatte
    0%   {color: #444}
    23%   {color: #159ec5}
    40%   {color: #b29c08;}
    60%   {color: #83468c;}
    100%  {color: #444;}
}

@keyframes glint2 {

  // darker versions of electric lettuce color palatte
    0%   {background-color: #444}
    23%   {background-color: #159ec5}
    40%   {background-color: #b29c08;}
    60%   {background-color: #83468c;}
    100%  {background-color: #444;}
}

//
// @keyframes flex {
//   0% {font-weight: 200;}
//   25% {font-weight: 400}
//   50% {font-weight: 900;}
//   75% {font-weight: 400;}
//   100% {font-weight: 200;}
// }

// @keyframes flex {
//   0% {font-weight: 900;}
//   25% {font-weight: 400}
//   50% {font-weight: 200;}
//   75% {font-weight: 400;}
//   100% {font-weight: 900;}
// }



</style>
