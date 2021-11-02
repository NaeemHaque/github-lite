<template>
  <div class="wrapper">
    <Toolbar/>
    <v-container v-if="loading">
      <LoadingBar />
    </v-container>
    <v-container v-if="!loading">
      <v-card
        class="mx-auto"
        max-width="900"
        tile
      >
        <v-img
          height="100%"
          width="900"
          src="https://cdn.vuetifyjs.com/images/cards/server-room.jpg"
        >
          <v-row
            align="end"
            class="fill-height text-center"
          >
            <v-col
              align-self="start"
              class="pa-0"
              cols="12"
            >
              <v-avatar size="250" style="margin-top: 5%" >
                <img
                  :src="data.userProfileImage"
                  alt="avater"
                >
              </v-avatar>
            </v-col>
            <v-col class="py-0">
              <v-list-item
                color="rgba(0, 0, 0, .4)"
                dark
              >
                <v-list-item-content>
                  <v-list-item-title class="text-h6">
                    {{ data.name }}
                  </v-list-item-title>
                  <div style="display: flex; justify-content: space-between;">
                    <h5 v-if="data.company">
                      <v-icon left color="blue-grey">mdi-domain</v-icon>
                      {{ data.company }}
                    </h5>
                    <h5 v-if="data.location">
                      <v-icon left color="blue-grey">mdi-map-marker</v-icon>
                      {{ data.location }}
                    </h5>
                    <h5 v-if="data.twitter">
                      <v-icon left color="blue-grey">mdi-twitter</v-icon>
                      {{ data.twitter }}
                    </h5>
                  </div>
                </v-list-item-content>
              </v-list-item>
            </v-col>
          </v-row>
        </v-img>
      </v-card>
      <v-row class="text-center">
        <v-col class="mb-2">
        <div style="text-align: center">
            <h4 v-if="data.bioData">BIO-DATA</h4>
            <p>
              <span class="blue-grey--text">
                  {{ data.bioData }}
              </span><br>
            </p>

        </div>

        </v-col>
      </v-row>
    </v-container>

    <v-container v-if="!loading">

      <div class="search-result mx-auto">

        <v-card outlined flat class="bg-none pa-5 white--text" >
          <v-row>
              <v-col cols="4" class="text-center repo-section">

                <router-link :to="'/profile/' + data.userName + '/repos'" class="decoration__none">
                  <v-card flat class="bg-none black--text">
                    <v-icon left dark>mdi-source-repository</v-icon>
                    Repositories({{ data.repos }})
                  </v-card>
                </router-link>

              </v-col>

              <v-col cols="4" class="text-center repo-section">

                <router-link :to="'/profile/' + data.userName + '/followers'" class="decoration__none">
                  <v-card flat class="bg-none black--text">
                    <v-icon left dark>mdi-account-star</v-icon>
                    Followers({{ data.followers }})
                  </v-card>
                </router-link>

              </v-col>

              <v-col cols="4" class="text-center repo-section">

                <router-link :to="'/profile/' + data.userName + '/following'" class="decoration__none">
                  <v-card flat class="bg-none black--text">
                    <v-icon left dark>mdi-account-multiple</v-icon>
                    <span>Following({{ data.following }})</span>
                  </v-card>
                </router-link>

              </v-col>

          </v-row>
        </v-card>
        <p class="body-1 blue-grey--text text-center my-3">Join Since : {{ data.date }} </p>
      </div>
    </v-container>

  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
import LoadingBar from '../../components/LoadingBar'
import Toolbar from '../../components/NavBar'

let name
export default {
  props: ['login'],

  data: () => ({
    username: '',
    data: [],
    loading: false
  }),

  components: {
    // toolbar,
    Toolbar,
    LoadingBar
  },

  created () {
    name = this.login
    this.loading = true
    axios.get('https://api.github.com/users/' + name)
      .then((res) => {
        this.data = this.prepareData(res.data)
        this.loading = false
      })
      .catch((error) => {
        this.loading = false
        console.log(error)
      })
  },
  methods: {
    prepareData (dataItems) {
      /* eslint-disable camelcase */
      const {
        avatar_url = '',
        login = '',
        name = '',
        company = '',
        location = '',
        twitter_username = '',
        bio = '',
        public_repos = '',
        followers = '',
        following = '',
        created_at = ''
      } = dataItems
      const templateObj = {}
      templateObj.userName = login || ''
      templateObj.name = name || ''
      templateObj.company = company || ''
      templateObj.twitter = twitter_username || ''
      templateObj.location = location || ''
      templateObj.bioData = bio || ''
      templateObj.repos = public_repos || ''
      templateObj.followers = followers || ''
      templateObj.following = following || ''
      templateObj.date = moment(String(created_at)).format('MM/DD/YYYY') || ''
      templateObj.userProfileImage = avatar_url || ''
      console.log(templateObj)
      return templateObj
    }
  }
}
</script>
