<template>
  <div class="wrapper">
    <v-container >
      <v-row class="text-center">
        <v-col cols="12">
          <v-avatar size="250">
            <img
              :src="data.avatar_url"
              alt="avater"
            >
          </v-avatar>
        </v-col>

        <v-col class="mb-2">
        <div>
          <h1 class="display-1 font-weight-bold white--text">
            {{ data.name }}
          </h1>

            <p>
              <span class="body-1 blue-grey--text">
                {{ data.login }}
              </span><br>
              <span class="headline blue-grey--text">
                  {{ data.bio }}
              </span><br>
              <span class="subtitle-1 blue-grey--text" v-if="data.company">
                  <v-icon left color="blue-grey">mdi-domain</v-icon>
                  {{ data.company }}
              </span><br>
              <span class="subtitle-1 blue-grey--text"  v-if="data.location">
                  <v-icon left color="blue-grey">mdi-map-marker</v-icon>
                  {{ data.location }}
              </span><br>
              <span class="subtitle-1 blue-grey--text"  v-if="data.twitter_username">
                  <v-icon left color="blue-grey">mdi-twitter</v-icon>
                  {{ data.twitter_username }}
              </span><br>
            </p>

        </div>

        </v-col>
      </v-row>
    </v-container>

    <v-container>

      <div class="search-result mx-auto">

        <v-card outlined flat class="bg-none pa-5 white--text" >
          <v-row>
              <v-col cols="4" class="text-center repo-section">

                <router-link :to="'/profile/' + data.login + '/repos'" class="decoration__none">
                  <v-card flat class="bg-none white--text">
                    <v-icon left dark>mdi-source-repository</v-icon>
                    Repositories({{ data.public_repos }})
                  </v-card>
                </router-link>

              </v-col>

              <v-col cols="4" class="text-center repo-section">

                <router-link :to="'/profile/' + data.login + '/followers'" class="decoration__none">
                  <v-card flat class="bg-none white--text">
                    <v-icon left dark>mdi-account-star</v-icon>
                    Followers({{ data.followers }})
                  </v-card>
                </router-link>

              </v-col>

              <v-col cols="4" class="text-center repo-section">

                <router-link :to="'/profile/' + data.login + '/following'" class="decoration__none">
                  <v-card flat class="bg-none white--text">
                    <v-icon left dark>mdi-account-multiple</v-icon>
                    <span>Following({{ data.following }})</span>
                  </v-card>
                </router-link>

              </v-col>

          </v-row>
        </v-card>
        <p class="body-1 blue-grey--text text-center my-3">Join Since : {{ date }} </p>
      </div>
    </v-container>

  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
let name
export default {
  props: ['login'],

  data: () => ({
    username: '',
    data: [],
    date: ''
  }),

  created () {
    name = this.login
    axios.get('https://api.github.com/users/' + name)
      .then((res) => {
        this.data = res.data
        // formate date
        this.date = moment(String(res.data.created_at)).format('MM/DD/YYYY')
      })
      .catch((error) => {
        console.log(error)
      })
  }
}
</script>
