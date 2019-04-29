<template>
  <v-app id="inspire">
    <blog-nav :drawer="drawer"></blog-nav>
    <v-toolbar color="indigo" dark fixed app>
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-toolbar-title>Blog</v-toolbar-title>
    </v-toolbar>
    <v-content>
      <v-container fluid fill-height>
        <v-layout
          justify-center
          align-center
        >
          <v-flex text-xs-center>
            <nuxt />
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>

  import BlogNav from '../components/navbar.vue';

  export default {
    
    components: {
      'blog-nav': BlogNav
    },

    props: {
      source: String
    },

    data() 
    {
      return {
          drawer: null,
      }
    },
    
    
  }
</script>