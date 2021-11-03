<template>
  <div class="wrapper">
    <Toolbar/>
    <v-container v-if="loading">
      <LoadingBar />
    </v-container>
    <v-container fluid v-if="!loading">
      <v-row class="text-center">
        <v-divider vertical class="my-5 mx-3" dark></v-divider>
        <v-col cols="12">
            <v-card flat outlined class="mt-5 bg-none pa-4">
                    <div>
                      <h1 class="text-center">Repositories</h1>
                        <v-row>
                          <v-col cols="12" md="6"  v-for="(item, i) in data" :key="i">
                            <v-card elevation="2" outlined class="mt-5 repo__card">
                                <a :href="item.htmlUrl" target="_blank" class="decoration__none">
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
                                        {{ item.watching }}
                                      </template>
                                      <span>Watchers</span>
                                  </v-tooltip>

                                  </v-col>
                                </v-row>

                                <p class="ml-4 mt-1 blue-grey--text caption">
                                  <span>Updated at: {{ item.date }}</span>
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

  created () {
    const name = this.login
    this.loading = true
    axios.get('https://api.github.com/users/' + name + '/repos')
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
          html_url = '',
          name = '',
          language = '',
          forks = '',
          watchers_count = '',
          description = '',
          updated_at = ''
        } = dataItem
        const templateObj = {}
        templateObj.htmlUrl = html_url || ''
        templateObj.name = name || ''
        templateObj.language = language || ''
        templateObj.forks = forks || ''
        templateObj.watching = watchers_count || ''
        templateObj.description = description || ''
        templateObj.date = moment(String(updated_at)).format('MM/DD/YYYY') || ''
        preparedData.push(templateObj)
        return templateObj
      })
      return preparedData
    }
  }
}
</script>
