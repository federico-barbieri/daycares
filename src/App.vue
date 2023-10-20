<template>
  <div>
    <h1>DAYCARES</h1>
      <section class="typesOfDaycares">
          <div class="kommunalDaycare">
                <h2>KOMMUNAL DAYCARES</h2>
                <p><strong>Number of kommunal daycares: {{ kommunalDaycares.length }}</strong></p>
                <ul v-for="daycare in kommunalDaycares" :key="daycare.properties.kkorgnr">
                  <li>
                      <h2>Name: {{ daycare.properties.enhedsnavn }}</h2>
                      <p>Enheder leader: {{ daycare.properties.enhedsleder.trim() !== '' ? daycare.properties.enhedsleder : "NO LEADER PROVIDED" }}</p>
                  </li>
                </ul>
            </div>

            <div class="privatDaycare">
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
  </div>

</template>

<script>

import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      daycares: [],
      kommunalDaycares: [],
      privatDaycares: []
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
  border: 1px solid red;
}

.privatDaycare{
  width: 50%;
  border: 1px solid red;
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
