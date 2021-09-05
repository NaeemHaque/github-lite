<template>

    <v-app-bar
      app
      color="#2D333B"
      dark
    >
    <router-link to="/" class="decoration__none ">

      <div class="d-flex align-center">
        <v-img
          alt="Github Logo"
          class="shrink mr-2"
          contain
          src="@/assets/images/github-logo-white.png"
          transition="scale-transition"
          width="40"
        />

        <h2>Github Assistant</h2>
      </div>
    </router-link>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/"
        target="_blank"
        text
      >
        <span class="mr-2">Explore Github</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

</template>
