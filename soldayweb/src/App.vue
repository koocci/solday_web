<template>
  <v-app id="inspire">
    <v-card class="overflow-hidden">
      <v-app-bar
        app
        absolute
        color="#6A76AB"
        dark
        shrink-on-scroll
        prominent
        src="https://picsum.photos/1920/1080?random"
        fade-img-on-scroll
        scroll-target="#content-sheet"
      >
        <template v-slot:img="{ props }">
          <v-img
            v-bind="props"
            gradient="to top right, rgba(100,115,201,.7), rgba(25,32,72,.7)"
          ></v-img>
        </template>
  
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
        <v-container id="main-container">
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
      const main_container = document.getElementById('main-container');
      console.log(main_container.offsetTop);
      document.getElementById(_id).scrollTop = main_container.offsetTop;
      // document.getElementById(_id).scrollIntoView(true);
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
