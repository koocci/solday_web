<template>
  <v-app id="inspire">

    <v-navigation-drawer
      v-model="drawer"
      app
    >
      <v-list dense>
        <v-list-item
          v-for="item in tabs"
          :key="item.id"
          @click="goToScroll(item.title, addScrollEvent); drawer = !drawer; selectedTab = item.id;"
        >
          <v-list-item-action>
            <v-icon>{{item.icon}}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{{item.title}}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      fixed
      color="#0C90AD"
      dark
      id="app-bar"
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>{{title}}</v-toolbar-title>

      <v-spacer></v-spacer>

      <template v-slot:extension>
        <v-tabs
          v-model="selectedTab"
          align-with-title
          grow
          center-active
          background-color="transparent"
        >
          <v-tab 
            v-for="item in tabs"
            :key="item.id"
            :id="'tab-' + item.id"
            @click="goToScroll(item.title, addScrollEvent);"
          >
            {{item.title}}
          </v-tab>
        </v-tabs>
      </template>
    </v-app-bar>
    <v-container fluid
      class="main-container"
    >
      <v-content>
        <Intro/>
        <Seminar/>
        <Booth/>
        <Map/>
      </v-content>
    </v-container>
      <v-footer
      dark
      padless
    >
      <v-card
        class="flex text-center"
        tile
      >
        <v-card-text class="teal" style="padding: 5px !important;">
          <strong class="subheading">Developed By 소프트웨어개발단</strong>
        </v-card-text>
        <v-card-actions class="grey darken-3 justify-center" style="padding: 3px !important;">
          {{ new Date().getFullYear() }} &nbsp; <strong>KT Solution Day.</strong>
        </v-card-actions>
      </v-card>
    </v-footer>
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
    drawer: false,
    selectedTab: null,
    tabs: [
      {
        id: 0,
        title: 'Intro',
        icon: 'fas fa-arrow-alt-circle-right'
      },
      {
        id: 1,
        title: 'Seminar',
        icon: 'fas fa-chalkboard-teacher'
      },
      {
        id: 2,
        title: 'Booth',
        icon: 'fas fa-chalkboard'
      },
      {
        id: 3,
        title: 'Map',
        icon: 'fas fa-map-marker-alt'
      }
    ],
    footer_icons: [
      'fab fa-facebook',
      'fab fa-twitter',
      'fab fa-google-plus',
      'fab fa-linkedin',
      'fab fa-instagram',
    ],
  }),
  methods: {
    goToScroll: function(title, callback) {
      this.removeScrollEvent();
      const onScroll = function () {
        const scrollTop = window.scrollTop || window.pageYOffset
        if (scrollTop === document.getElementById(title).offsetTop) {
            window.removeEventListener('scroll', onScroll)
            callback()
        }
      }

      window.addEventListener('scroll', onScroll)
      window.scrollTo({
        top: document.getElementById(title).offsetTop,
        behavior: "smooth"
      });
    },
    setActiveTabChangeByScroll: function() {
      let tabOffsetTops = Array();
      for (let item of this.tabs) {
        tabOffsetTops.push(document.getElementById(item.title).offsetTop);
      }
      let index = 0;
      let lastTabFlag = true;

      while (index < tabOffsetTops.length -1) {
        if (tabOffsetTops[index] <= window.scrollY && window.scrollY < tabOffsetTops[index+1]){
          lastTabFlag = false;
          break; 
        }
        index ++;
      }
      if (lastTabFlag) {
        this.selectedTab = tabOffsetTops.length - 1;
      }
      else {
        this.selectedTab = index;
      }
    },
    addScrollEvent: function() {
      window.addEventListener('scroll', this.setActiveTabChangeByScroll);
    },
    removeScrollEvent: function() {
      window.removeEventListener('scroll', this.setActiveTabChangeByScroll);
    }
  },
  created: function() {
    this.addScrollEvent();
  },
  beforeDestroy: function() {
    this.removeScrollEvent();
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
.main-container {
  padding: 0px !important;
}
</style>
