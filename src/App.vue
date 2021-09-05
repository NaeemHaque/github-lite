<template>
  <v-app>
    <toolbar />

    <v-main>
      <router-view/>
    </v-main>

    <Footeer />

  </v-app>
</template>

<script>
import toolbar from './components/Toolbar'
import Footeer from './components/Footer'

export default {
  name: 'App',

  data: () => ({
    //
  }),
  components: {
    toolbar,
    Footeer
  }
}
</script>
