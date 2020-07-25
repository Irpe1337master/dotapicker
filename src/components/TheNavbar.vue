<template>
  <v-app-bar
      color="primary"
      dense
      dark
    >
    <v-toolbar-title>
      <router-link to="/" tag="span" style="cursor: pointer">
        {{ appTitle }}
      </router-link>
    </v-toolbar-title>
    <v-toolbar-items class="hidden-xs-only">
      <v-btn
        text
        v-for="item in links"
        :key="item.text"
        :to="item.path">
        <v-icon left>{{ item.icon }}</v-icon>
        {{ item.text }}
      </v-btn>
    </v-toolbar-items>
  </v-app-bar>
</template>

<script>

  
export default {
  name: 'Navbar',
  data: () => ({ 
    appTitle: 'Coolest Äppi',
    links: [
      {
        path: "/",
        text: "Home",
        icon: "home"
      },
      {
        path: "/different-api",
        text: "Different api",
        icon: "3d_rotation"
      },
      {
        path: "/about",
        text: "About",
        icon: "face"
      }
    ]
  })
  
};
</script>
