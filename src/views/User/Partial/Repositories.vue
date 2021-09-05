<template>
  <div class="wrapper">
    <v-container fluid>
      <v-row>
        <v-col cols="3" class="mx-auto">
            <v-row class="mt-7">
            <v-col cols="12" class="ml-5">
              <v-avatar size="200">
                <img  :src="user.avatar_url" alt="avater" />
              </v-avatar>
            </v-col>

            <v-col class="mb-2">
              <div>
                <h2 class="headline font-weight-bold white--text">{{  user.name }}</h2>

                <p>
                  <span class="body-2 blue-grey--text">{{ user.login }}</span><br />

                  <span class="subtitile-1 blue-grey--text"> {{ user.bio }} </span><br />
                  <span class="subtitle-2 blue-grey--text" v-if="user.company">
                    <v-icon left color="blue-grey">mdi-domain</v-icon>
                    {{ user.company }}
                  </span
                  ><br/>
                  <span class="subtitle-2 blue-grey--text" v-if="user.location">
                    <v-icon left color="blue-grey">mdi-map-marker</v-icon>
                    {{ user.location }}
                  </span
                  ><br/>
                  <span class="subtitle-2 blue-grey--text" v-if="user.twitter_username">
                    <v-icon left color="blue-grey">mdi-twitter</v-icon>
                    {{ user.twitter_username }}
                  </span
                  ><br/>
                </p>
              </div>
            </v-col>
          </v-row>
        </v-col>
        <v-divider vertical class="my-5 mx-3" dark></v-divider>
        <v-col cols="8">
            <v-card flat outlined class="mt-5 bg-none pa-4">

                    <h2 class="text-center white--text mt-3 mb-6"> {{ user.public_repos }} Repositories</h2>

                    <div>
                        <v-row>
                          <v-col cols="12" md="6"  v-for="(item, i) in data" :key="i">
                            <v-card flat outlined class="mt-5 repo__card">
                                <a :href="item.html_url" target="_blank" class="decoration__none">
                                  <h4 class="mt-3 ml-3 blue--text">{{item.name}}</h4>
                                </a>
                                <v-row class="mx-1">

                                  <v-col cols="4" class="blue-grey--text caption" v-if="item.language">
                                    <v-tooltip bottom small>
                                      <template v-slot:activator="{ on, attrs }">
                                        <v-icon
                                          left
                                          small
                                          color="blue-grey"
                                          v-bind="attrs"
                                          v-on="on"
                                        >
                                          mdi-file-code
                                        </v-icon>
                                        {{ item.language }}
                                      </template>
                                      <span>Language</span>
                                    </v-tooltip>
                                  </v-col>

                                  <v-col cols="4" class="blue-grey--text caption">
                                    <v-tooltip bottom small>
                                      <template v-slot:activator="{ on, attrs }">
                                        <v-icon
                                          left
                                          small
                                          color="blue-grey"
                                          v-bind="attrs"
                                          v-on="on"
                                        >
                                          mdi-source-fork
                                        </v-icon>
                                        {{ item.forks }}
                                      </template>
                                      <span>Forks</span>
                                    </v-tooltip>
                                  </v-col>

                                  <v-col cols="4" class="blue-grey--text caption">

                                    <v-tooltip bottom small>
                                      <template v-slot:activator="{ on, attrs }">
                                        <v-icon
                                          left
                                          small
                                          color="blue-grey"
                                          v-bind="attrs"
                                          v-on="on"
                                        >
                                          mdi-eye
                                        </v-icon>
                                        {{ item.watchers_count }}
                                      </template>
                                      <span>Watchers</span>
                                  </v-tooltip>

                                  </v-col>
                                </v-row>

                                <p class="ml-4 mt-1 blue-grey--text caption">
                                  <span>Updated at: {{ date[i] }}</span>
                                </p>
                                <p class="ml-4 blue-grey--text caption">
                                  <span> {{ item.description }} </span>
                                </p>

                            </v-card>
                         </v-col>
                        </v-row>
                    </div>
            </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'

export default {
  props: ['login'],

  data: () => ({
    username: '',
    data: [],
    user: [],
    date: []
  }),

  created () {
    const name = this.login
    axios.get('https://api.github.com/users/' + name + '/repos')
      .then((res) => {
        this.data = res.data
        // formate date
        for (var i in res.data) {
          this.date.push(moment(String(res.data[i].updated_at)).format('MM/DD/YYYY'))
        }
      })
      .catch((error) => {
        console.log(error)
      })

    axios.get('https://api.github.com/users/' + name)
      .then((res) => {
        this.user = res.data
      })
      .catch((error) => {
        console.log(error)
      })
  }
}
</script>
