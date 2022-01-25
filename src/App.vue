<template>
   <v-app>
      <v-app-bar color="grey darken-3">
         <v-spacer></v-spacer>
         <h2>Vuenice</h2>
         <v-icon right>mdi-penguin</v-icon>
         <v-spacer></v-spacer>
      </v-app-bar>
      <Main />
      <v-footer>
         <div class="d-flex justify-center align-center" style="width: 100%">
            <p style="margin: 0">{{ new Date().getFullYear() }} —</p>
            <v-icon small>mdi-copyright</v-icon>
            <a href="http://jfourc.com" target="_blank" rel="noopener noreferrer">{{ "J4C" }}</a>
         </div>
      </v-footer>
   </v-app>
</template>

<script>
import Main from "./components/Main.vue";

export default {
   name: "App",
   components: {
      Main,
   },
};
</script>
