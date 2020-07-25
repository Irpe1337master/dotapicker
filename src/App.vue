<template>
  <v-app>
    <TheNavbar />
    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>

import TheNavbar from '@/components/TheNavbar'
  
export default {
  name: 'App',
  components: { TheNavbar }
  
};
</script>
