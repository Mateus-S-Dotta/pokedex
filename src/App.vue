<template>
  <div class="content">
    <CardVue />
    <CardVue />
    <CardVue />
    <CardVue />
    <CardVue />
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