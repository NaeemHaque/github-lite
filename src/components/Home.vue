<template>
<div class="wrapper">
  <v-container >
    <v-row class="text-center">
      <v-col cols="12">
        <v-img
          src="@/assets/images/github-logo-white.png"
          class="my-3"
          contain
          height="200"
        />
      </v-col>

      <v-col class="mb-2">
        <h1 class="display-2 font-weight-bold white--text">
          Welcome to Github Assistant
        </h1>

      </v-col>
    </v-row>

    <v-row class="text-center">
      <form v-on:submit.prevent="submitForm" class="mx-auto text-center">

          <v-row>
            <v-text-field
            label="Search Name"
            outlined
            dark
            class="mx-auto mt-3 text--field"
            append-icon="mdi-magnify"
            v-model="username"
          ></v-text-field>
          </v-row>

          <v-row>
            <v-btn
              class="pa-3 search mx-auto"
              outlined
              dark
              type="submit"
            >
              Search
            </v-btn>
          </v-row>

      </form>
    </v-row>

  </v-container>

  <v-container>
    <div class="search-result mx-auto">
      <v-card outlined flat class="bg-none py-4" v-if="clicksubmit">
        <h2 class="text-center white--text my-3">Search Result</h2>
        <v-row class="mx-3">
          <v-col cols="12" sm="6" class="mx-auto" v-for="item in dataItems" :key="item.id">
            <router-link :to="'/profile/' + item.login" class="decoration__none">
              <v-card flat outlined class="pa-3 search--item">
                <v-row>
                  <v-col cols="2">
                    <v-avatar>
                      <img
                        :src="item.avatar_url"
                        alt="avater"
                      >
                    </v-avatar>
                  </v-col>
                  <v-col cols="9" class="my-auto ml-3">
                    <h3 class="white--text px-2">{{ item.login }}</h3>
                  </v-col>
                </v-row>
              </v-card>
            </router-link>
          </v-col>
        </v-row>
      </v-card>
    </div>
  </v-container>

  </div>
</template>

<script>
import axios from 'axios'

export default {

  data: () => ({
    username: '',
    clicksubmit: '',
    data: [],
    dataItems: []
  }),

  methods: {
    submitForm () {
      let name = this.username
      this.clicksubmit = this.username
      name = name.split(' ').join('')

      axios.get('https://api.github.com/search/users?q=' + name, this.form)
        .then((res) => {
          this.data = res.data
          this.dataItems = res.data.items
        })
        .catch((error) => {
          console.log(error)
        })
    }

  }

}
</script>

<style scoped>
.v-text-field--outlined >>> fieldset {
  border: 2px solid indigo !important;
}
.search.v-btn--outlined {
  border: 2px solid indigo !important;
}
</style>
