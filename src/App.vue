<template>
  <v-app>
    <v-main>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/WeatherForecast.vue'

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
}
</script>

<style scoped>

.v-main{ 
  background-color: rgb(144, 187, 223)
}

</style>
