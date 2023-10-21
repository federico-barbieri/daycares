<template>
  <main>
    <h1>DAYCARES IN COPENHAGEN</h1>
    <div class="btns-div">
      <button @click="showPrivateDaycares">Private Daycares</button>
      <button @click="showKommunalDaycares">Kommunal Daycares</button>

    </div>
      <section class="typesOfDaycares">
          <div class="kommunalDaycare" v-if="kommunalDaycareIsClicked">
                <h2>KOMMUNAL DAYCARES</h2>
                <p><strong>Number of kommunal daycares: {{ kommunalDaycares.length }}</strong></p>
                <ul v-for="daycare in kommunalDaycares" :key="daycare.properties.kkorgnr">
                  <li>
                      <h2>Name: {{ daycare.properties.enhedsnavn }}</h2>
                      <p>Enheder leader: {{ daycare.properties.enhedsleder.trim() !== '' ? daycare.properties.enhedsleder : "NO LEADER PROVIDED" }}</p>
                  </li>
                </ul>
            </div>

            <div class="privatDaycare" v-if="privateDaycareIsClicked">
                <h2>PRIVAT DAYCARES</h2>
                <p><strong>Number of privat daycares: {{ privatDaycares.length }}</strong></p>
                <ul v-for="daycare in privatDaycares" :key="daycare.properties.kkorgnr">
                    <li>
                        <h2>Name: {{ daycare.properties.enhedsnavn }}</h2>
                        <p>Enheder leader: {{ daycare.properties.enhedsleder.trim() !== '' ? daycare.properties.enhedsleder : "NO LEADER PROVIDED" }}</p>
                    </li>
                </ul>
            </div>
    </section>
  </main>

</template>

<script>

import axios from 'axios';
import { ref } from 'vue';

export default {
 
  name: 'App',
  data() {
    return {
      daycares: [],
      kommunalDaycares: [],
      privatDaycares: [],
      kommunalDaycareIsClicked: ref(false),
      privateDaycareIsClicked: ref(false),
    };
  },
  mounted() {
    axios.get('/daycares.json') // Assuming daycares.json is in the public folder
      .then(response => {
        this.daycares = response.data.features;
        this.kommunalDaycares = this.daycares.filter(daycare => daycare.properties.ejerforhold.toLowerCase() === "kommunal");
        this.privatDaycares = this.daycares.filter(daycare => daycare.properties.ejerforhold.toLowerCase() === "selvejende");

      })
      .catch(error => {
        console.error('Error fetching data:', error);
      });
  },
  methods: {
    showKommunalDaycares(){
      this.kommunalDaycareIsClicked = !this.kommunalDaycareIsClicked;
    },
    showPrivateDaycares(){
      this.privateDaycareIsClicked = !this.privateDaycareIsClicked;
    },
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

main{
  width: 100vw;
  height: 100vh;
  text-align: center;
}

.btns-div{
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-around;
  width: 50%;
  height: auto;
  margin: 5rem auto;
}

.btns-div > button{
  padding: 1rem;
}


.typesOfDaycares{
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  justify-content: space-around;
  width: 90vw;
  margin: 0 auto;
}

.kommunalDaycare{
  width: 50%;
  border: 1px solid #2c3e50;
  border-radius: 30px;
}

.privatDaycare{
  width: 50%;
  border: 1px solid #2c3e50;
  border-radius: 30px;
}


ul{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  height: auto;
  margin: 0 auto;
  list-style-type: none;
}

ul > li {
  padding: 1rem;
  text-align: center;
  border: 1px solid black;
  border-radius: 10px;
  transition: all 0.5s ease-in;
  margin: 1rem auto;
  width: 80%;

}

ul > li:hover {
  padding: 1rem;
  text-align: center;
  border: 1px solid black;
  background-color: black;
  color: white;
  border-radius: 0;
  transition: all 0.5s ease-in;
  cursor: pointer;
}

</style>
