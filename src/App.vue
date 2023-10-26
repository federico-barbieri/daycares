<template>
  <main>
    <h1>DAYCARES IN COPENHAGEN</h1>
    <div class="btns-div">
      <button @click="showSelfOwnedDaycares">Kommunal Daycares (self-owned)</button>
      <button @click="showKommunalDaycares">Kommunal Daycares</button>
      <button @click="showPrivateDaycares">Private Daycares</button>


    </div>
      <section class="typesOfDaycares">
          <div class="kommunalDaycare" v-if="kommunalDaycareIsClicked">
                <h3>KOMMUNAL DAYCARES (kommunal)</h3>
                <p><strong>Number of kommunal daycares: {{ kommunalDaycares.length }}</strong></p>
                <ul>
                  <li v-for="daycare in kommunalDaycares" :key="daycare.properties.kkorgnr">
                    <DaycareCard
                    :name="daycare.properties.enhedsnavn"
                    :address="daycare.properties.adresse"
                    :district="daycare.properties.postnr_og_postdistrikt" 
                    :type="daycare.properties.ejerforhold"
                    />
                  </li>
                </ul>
            </div>

            <div class="privatDaycare" v-if="privateDaycareIsClicked">
                <h3>KOMMUNAL DAYCARES (self-owned)</h3>
                <p><strong>Number of self-owned kommunal daycares: {{ privatDaycares.length }}</strong></p>
                <ul >
                    <li v-for="daycare in privatDaycares" :key="daycare.properties.kkorgnr">
                      <DaycareCard
                    :name="daycare.properties.enhedsnavn"
                    :address="daycare.properties.adresse"
                    :district="daycare.properties.postnr_og_postdistrikt" 
                    :type="daycare.properties.ejerforhold"
                    />
                    </li>
                </ul>
            </div>

    </section>
  </main>

</template>

<script>

import axios from 'axios';
import { ref } from 'vue';
// eslint-disable-next-line no-unused-vars
import DaycareCard from './components/DaycareCard.vue';

export default {
  components: {
    DaycareCard,
  },
 
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
    showSelfOwnedDaycares(){
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
  cursor: pointer;
  transition: all 0.5s ease-in;
}

.btns-div > button:hover{
  padding: 1rem;
  cursor: pointer;
  background-color: black;
  color: white;
}

ul{
  padding-inline-start: 0px;
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
  max-width: 30%;
  border: 1px solid #2c3e50;
  border-radius: 30px;
  height: 50vh;
  overflow-y: scroll;


}

.privatDaycare{
  max-width: 30%;
  height: 50vh;
  border: 1px solid #2c3e50;
  border-radius: 30px;
  overflow-y: scroll;
}

.emails{
  max-width: 30%;
  height: 50vh;
  border: 1px solid #2c3e50;
  border-radius: 30px;
  overflow-y: scroll;
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

@media screen and (max-width: 1300px) {
      .typesOfDaycares{
          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: space-around;
          width: 100vw;
          margin: 0 auto;
    }

    .btns-div{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  width: 100%;
  height: auto;
  margin: 5rem auto;
}

.btns-div > button{
  padding: 1rem;
  cursor: pointer;
  transition: all 0.5s ease-in;
  margin: 1rem auto;
}

.btns-div > button:hover{
  padding: 1rem;
  cursor: pointer;
  background-color: black;
  color: white;
}

ul{
  padding-inline-start: 0px;
}


.typesOfDaycares{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  width: 100vw;
  margin: 2rem auto;
}

.kommunalDaycare{
  max-width: 90%;
  border: 1px solid #2c3e50;
  border-radius: 30px;
  height: 50vh;
  overflow-y: scroll;
  margin: 2rem auto;

}

.privatDaycare{
  max-width: 90%;
  height: 50vh;
  border: 1px solid #2c3e50;
  border-radius: 30px;
  overflow-y: scroll;
  margin: 2rem auto;

}

.emails{
  max-width: 90%;
  height: 50vh;
  border: 1px solid #2c3e50;
  border-radius: 30px;
  overflow-y: scroll;
  margin: 2rem auto;

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

}

</style>
