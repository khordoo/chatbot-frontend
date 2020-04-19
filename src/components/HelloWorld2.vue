<template>
  <v-app id="sandbox">
    <!-- <v-navigation-drawer
      v-model="primaryDrawer.model"
      :clipped="primaryDrawer.clipped"
      :floating="primaryDrawer.floating"
      :mini-variant="primaryDrawer.mini"
      :permanent="primaryDrawer.type === 'permanent'"
      :temporary="primaryDrawer.type === 'temporary'"
      app
      overflow
    />-->
    <v-navigation-drawer
      v-model="primaryDrawer.model"
      class="chatbar--gradient"
      app
      left
      :clipped="false"
      permanent="true"
      :mini-variant.sync="open"
      mini-variant-width="70"
      width="250px"
    >
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

    <v-app-bar :clipped-left="primaryDrawer.clipped" app>
      <v-app-bar-nav-icon
        v-if="primaryDrawer.type !== 'permanent'"
        @click.stop="primaryDrawer.model = !primaryDrawer.model"
      />
      <v-toolbar-title>RL Chatbot</v-toolbar-title>
    </v-app-bar>
    <v-container class="grey lighten-5" fluid="true">
      <v-row no-gutters>
        <v-col v-for="n in 1" :key="n" cols="12" sm="12">
          <v-card class="pa-2" outlined tile height="700">
            <v-list>
              <v-subheader>CONVERSATION</v-subheader>
              <v-list-item-group v-model="item" color="primary">
                <v-list-item v-for="(dialog, i) in dialogs" :key="i">
                  <v-list-item-avatar>
                    <img :src="`https://randomuser.me/api/portraits/men/${dialog.user.iconIndex}.jpg`" alt />
                  </v-list-item-avatar>
                  <v-list-item-content>
                    <v-list-item-title v-text="dialog.user.text" ></v-list-item-title>
                    <v-list-item-title v-text="dialog.bot.text" ></v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-list-item-group>
            </v-list>
          </v-card>
        </v-col>
      </v-row>
      <v-row no-gutters>
        <v-col v-for="n in 1" :key="n" cols="12" sm="12">
          <!-- <v-card class="pa-2 fill" outlined tile  height="50">One of three columns</v-card> -->
          <v-text-field
            :append-icon-cb="() => {}"
            placeholder="Start typing..."
            single-line
            append-icon="mdi-send"
            color="gray"
            hide-details
          />
        </v-col>
      </v-row>
    </v-container>

    <v-footer :inset="footer.inset" app>
      <span class="px-4">&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    drawers: ["Default (no property)", "Permanent", "Temporary"],
    primaryDrawer: {
      model: null,
      type: "default (no property)",
      clipped: false,
      floating: false,
      mini: false
    },
    items2: [
      { picture: 28, text: "Bot" },
      { picture: 78, text: "User" }
    ],
    dialogs: [
      {
        user: { iconIndex: 28, text: "How are you?" },
        bot: { iconIndex: 78, text: "Never been better!" }
      },
      {
        user: { iconIndex: 28, text: "Really?" },
        bot: { iconIndex: 78, text: "Yep!" }
      }
    ],
    footer: {
      inset: false
    }
  })
};
</script>