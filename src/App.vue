<template>
  <div style="width: 70%; margin: 0 auto;">
    <div style="width: 70%; margin: 0 auto; border: 1px solid black;">
      Questo progetto consente di esplorare interattivamente i dati relativi a 3085 pozzi di trivellazioni studiati in Italia.
      Clicca su Mappa per visualizzare la carta geografica e localizzare i pozzi con i loro attributi.
      Clicca su Grafici e statistiche per interagire con i dati, visualizzandone alcune proprietà interessanti.
    </div>
    <div>
      <button v-on:click="component = 'Dashboard'" id="Dashboard">Mappa</button>
      <button v-on:click="component = 'Visualizzazioni'" id="Visualizzazioni">Grafici e Statistiche</button>
    </div>
    <keep-alive>
      <component v-bind:is="component"></component>
    </keep-alive>
  </div>
  
</template>

<script>
//import { component } from 'vue/types/umd'
import Dashboard from './components/Dashboard.vue'
import Visualizzazioni from './components/Visualizzazioni.vue'
export default {
  name: 'App',
  components: {
    Dashboard,
    Visualizzazioni
  },
  data() {
    return{
      component: 'Dashboard'
      
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #bd573d;
}
</style>
