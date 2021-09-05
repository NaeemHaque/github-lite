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
                  <span class="body-2 blue-grey--text">{{ user.login }}</span><br/>
                  <span class="subtitile-1 blue-grey--text"> {{ user.bio }} </span><br/>

                  <span class="subtitle-2 blue-grey--text" v-if="user.company">
                    <v-icon left color="blue-grey">mdi-domain</v-icon>
                    {{ user.company }}
                  </span
                  ><br />
                  <span class="subtitle-2 blue-grey--text" v-if="user.location">
                    <v-icon left color="blue-grey">mdi-map-marker</v-icon>
                    {{ user.location }}
                  </span
                  ><br />
                  <span class="subtitle-2 blue-grey--text" v-if="user.twitter_username">
                    <v-icon left color="blue-grey">mdi-twitter</v-icon>
                    {{ user.twitter_username }}
                  </span
                  ><br />
                </p>
              </div>
            </v-col>
          </v-row>
        </v-col>
        <v-divider vertical class="my-5 mx-3" dark></v-divider>
        <v-col cols="8">
            <v-card flat outlined class="mt-5 bg-none pa-4">

                  <h2 class="text-center white--text mt-3 mb-6"> {{ user.followers }} Followers</h2>

                    <div>
                        <v-row>
                          <v-col cols="12" md="6"  class="mx-auto" v-for="item in data" :key="item.login">
                            <router-link :to="'/profile/' + item.login" class="decoration__none">
                              <v-card flat outlined class="mt-5 pa-4 repo__card search--item">
                                  <v-row>
                                      <v-col cols="2">
                                        <v-avatar>
                                          <img
                                              :src="item.avatar_url"
                                              alt="avater"
                                          >
                                          </v-avatar>
                                      </v-col>
                                      <v-col cols="10">
                                          <h2 class="headline mt-2 blue-grey--text"> {{ item.login }} </h2>
                                      </v-col>
                                  </v-row>
                              </v-card>
                            </router-link>
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

export default {
  props: ['login'],

  data: () => ({
    username: '',
    data: [],
    user: []
  }),
  computed: {

  },

  created () {
    const name = this.login
    axios.get('https://api.github.com/users/' + name + '/followers')
      .then((res) => {
        this.data = res.data
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
