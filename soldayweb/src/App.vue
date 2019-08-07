<template>
  <v-app id="inspire">
    <v-card class="overflow-hidden">
      <v-app-bar
        app
        fixed
        color="#0C90AD"
        dark
        id="app-bar"
      >
        <v-app-bar-nav-icon></v-app-bar-nav-icon>

        <v-toolbar-title>{{title}}</v-toolbar-title>

        <v-spacer></v-spacer>
  
        <template v-slot:extension>
          <v-tabs
            v-model="selectedTab"
            align-with-title
            background-color="transparent"
          >
            <v-tab 
              v-for="item in tabs"
              :key="item.tab_id"
              :id="'tab-' + item.tab_id"
              @click="goToScroll(item.title)"
            >
              {{item.title}}
            </v-tab>
          </v-tabs>
        </template>
      </v-app-bar>
      <v-sheet
        id="content-sheet"
        class="overflow-y-auto"
      >
        <v-container
        >
          <v-content>
            <Intro/>
            <Seminar/>
            <Booth/>
            <Map/>
          </v-content>
        </v-container>
      </v-sheet>
    </v-card>

  </v-app>
</template>

<script>
import Intro from './components/Intro';
import Seminar from './components/Seminar';
import Booth from './components/Booth';
import Map from './components/Map';

export default {
  name: 'App',
  components: {
    Intro, Seminar, Booth, Map
  },
  data: () => ({
    title: '전지적 참관 시점',
    selectedTab: null,
    tabs: [
      {
        tab_id: 1,
        title: 'Intro'
      },
      {
        tab_id: 2,
        title: 'Seminar'
      },
      {
        tab_id: 3,
        title: 'Booth'
      },
      {
        tab_id: 4,
        title: 'Map'
      }
    ]
  }),
  methods: {
    goToScroll: (_id) => {
      window.scrollTo({
        top: document.getElementById(_id).offsetTop,
        behavior: "smooth"
      });
    },
  }
};
</script>

<style>
html, body {
  scroll-behavior: smooth;
  height: 100%;
}
.components-size {
  height: 1000px;
}
</style>
