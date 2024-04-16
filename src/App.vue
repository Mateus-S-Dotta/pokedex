<template>
  <div class="content">
    <CardVue pokemonId="1" />
    <CardVue pokemonId="2" />
    <CardVue pokemonId="3" />
    <CardVue pokemonId="4"/>
    <CardVue pokemonId="5"/>
    <CardVue pokemonId="6"/>
    <CardVue pokemonId="7"/>
    <CardVue pokemonId="8"/>
    <CardVue pokemonId="9"/>
  </div>
</template>

<script>
import CardVue from './components/CardVue.vue'

export default {
  name: 'App',
  components: {
    CardVue
  }
}
</script>

<style>
body {
  background-color: #282828;
}
  html {
    font-size: 62.5%;
  }

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .content {
    display: flex;
    gap: 3.2rem;
    justify-content: center;
    flex-flow: row wrap;
    overflow: hidden;
    padding: 3.2rem;
  }
</style>