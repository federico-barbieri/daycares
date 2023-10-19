<template>
  <div>
    <h1>KOMMUNAL DAYCARES</h1>
    <p><strong>Number of kommunal daycares: {{ filteredDaycares.length }}</strong></p>
    <ul v-for="daycare in filteredDaycares" :key="daycare.properties.kkorgnr">
      <li>
          <h2>Name: {{ daycare.properties.enhedsnavn }}</h2>
          <p>Enheder leader: {{ daycare.properties.enhedsleder.trim() !== '' ? daycare.properties.enhedsleder : "NO LEADER PROVIDED" }}</p>
      </li>
    </ul>
  </div>

</template>

<script>

import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      daycares: [],
      filteredDaycares: []
    };
  },
  mounted() {
    axios.get('/daycares.json') // Assuming daycares.json is in the public folder
      .then(response => {
        this.daycares = response.data.features;
        this.filteredDaycares = this.daycares.filter(daycare => daycare.properties.ejerforhold.toLowerCase() === "kommunal");
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


ul{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  max-width: 80vw;
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
