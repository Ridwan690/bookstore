<template>
  <v-app>
      <c-header />

      <c-side-bar />

      <v-content>
          <v-slide-y-transition mode="out-in">
              <router-view></router-view>
          </v-slide-y-transition>
      </v-content>
      
      <c-footer />
  </v-app>
  
</template>
<script>
      import CHeader from '@/components/CHeader.vue';
      import CFooter from '@/components/CFooter.vue';
      import CSideBar from '@/components/CSideBar.vue';

      

      export default {
          name: 'App',
          components: {
              CHeader,
              CFooter,
              CSideBar,
          },
      }
</script>
<style type="text/css">
  .v-toolbar {
      flex: 0 !important;
  }
  .v-application .py-3 {
      text-align: center !important;
  }
  .v.card__text {
      text-align: center !important;
  }
</style>