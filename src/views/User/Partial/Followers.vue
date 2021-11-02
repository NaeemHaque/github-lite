<template>
  <div class="wrapper">
    <Toolbar/>
    <v-container v-if="loading">
      <LoadingBar />
    </v-container>
    <v-container fluid v-if="!loading">
      <v-row>
        <v-col cols="12">
            <v-card flat outlined class="mt-5 bg-none pa-4">

                    <div>
                        <v-row>
                          <v-col cols="12" md="6"  class="mx-auto" v-for="item in data" :key="item.userName">
                            <router-link :to="'/profile/' + item.userName" class="decoration__none">
                              <v-card flat outlined class="mt-5 pa-4 repo__card search--item">
                                  <v-row>
                                      <v-col cols="2">
                                        <v-avatar>
                                          <img
                                              :src="item.userProfileImage"
                                              alt="avater"
                                          >
                                          </v-avatar>
                                      </v-col>
                                      <v-col cols="10">
                                          <h2 class="headline mt-2 blue-grey--text"> {{ item.userName }} </h2>
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
import Toolbar from '../../../components/NavBar'
import LoadingBar from '../../../components/LoadingBar'
import _ from 'lodash'

export default {
  props: ['login'],

  data: () => ({
    data: [],
    loading: false
  }),

  components: {
    // toolbar,
    Toolbar,
    LoadingBar
  },
  computed: {

  },

  created () {
    this.loading = true
    const name = this.login
    axios.get('https://api.github.com/users/' + name + '/followers')
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
      const preparedData = []
      _.map(dataItems, (dataItem) => {
        /* eslint-disable camelcase */
        const {
          avatar_url = '',
          login = ''
        } = dataItem
        const templateObj = {}
        templateObj.userName = login || ''
        templateObj.userProfileImage = avatar_url || ''
        preparedData.push(templateObj)
        return templateObj
      })
      return preparedData
    }
  }
}
</script>
