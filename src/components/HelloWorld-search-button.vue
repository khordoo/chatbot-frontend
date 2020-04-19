<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app>
      <!-- <v-list dense>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-home</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Home</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-contact-mail</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Contact</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>-->
      <v-list dense>
        <v-subheader class="mt-4 grey--text text--darken-1">Online users</v-subheader>
        <v-list>
          <v-list-item v-for="item in items2" :key="item.text" link>
            <v-list-item-avatar>
              <img :src="`https://randomuser.me/api/portraits/men/${item.picture}.jpg`" alt />
            </v-list-item-avatar>
            <v-list-item-title v-text="item.text" />
          </v-list-item>
        </v-list>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app color="indigo" dark>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>RL Enhanced Chatbot</v-toolbar-title>
    </v-app-bar>

    <v-content >
      <v-container class="fill-height" fluid>
        

        <v-row align="left" justify="left">
          <v-text-field
            flat
            full-width="true"
            solo-inverted
            hide-details
            prepend-inner-icon="mdi-send"
            label="Start chatting..."
            class="hidden-sm-and-down"
          />
        </v-row>
      </v-container>
    </v-content>
    <v-footer color="indigo" app>
      <span class="white--text">&copy; Create by Mahmood Khordoo @2020</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  props: {
    source: String
  },

  data: () => ({
    drawer: null,
    items2: [
      { picture: 28, text: "Joseph" },
      { picture: 78, text: "MKBHD" }
    ]
  })
};
</script>