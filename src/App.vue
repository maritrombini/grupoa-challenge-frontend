<template>
<v-app  id="inspire" class="bg-white" >
    <v-navigation-drawer   v-model="drawer" app >
      
      <!-- inicio area lateral -->         
    
      <v-img class="bg-white"
        lazy-src="https://yt3.ggpht.com/ytc/AKedOLTRHkFAntBVL8_S69lb7I6p6eHSltWFXMpsaGEgxA=s900-c-k-c0x00ffffff-no-rj"
        max-height="200"
        max-width="2550"  
        src="https://yt3.ggpht.com/ytc/AKedOLTRHkFAntBVL8_S69lb7I6p6eHSltWFXMpsaGEgxA=s900-c-k-c0x00ffffff-no-rj">
      </v-img>
  
        <div   class="card" style="width: 15.8rem" >  
          <div  class="card-body bg-danger pr-5">
            <blockquote class="blockquote" >
              <p  class="text-left">"É uma história contada com reticências de uma pessoa que construiu uma empresa grande e muito importante, a partir do zero, e cuja grandeza e importância ultrapassaram suas maiores expectativas."</p>
                  <footer class="blockquote-footer"><cite title="">Henrique L Kipperman</cite></footer>
            </blockquote>
            </div>
          </div>    
   
    </v-navigation-drawer>

    
  <!-- <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar> -->

    <v-main>
      <router-view></router-view>
      <!-- <HelloWorld/> -->
    </v-main>
  </v-app>
</template>

<script>
//import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  data: () => ({
    drawer: null,
  }),
};
</script>
