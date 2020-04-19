
<template>
  <v-app id="sandbox" app color="cyan" dark>
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
              <img :src="`https://randomuser.me/api/portraits/${item.picture}.jpg`" alt />
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
            <v-list two-line="true">
              <v-subheader>CONVERSATION</v-subheader>
              <v-list-item-group color="primary">
                <v-list-item v-for="(dialog) in dialogs" :key="dialog.index">
                  <v-list-item-avatar>
                    <img :src="`https://randomuser.me/api/portraits/${dialog.iconIndex}.jpg`" alt />
                  </v-list-item-avatar>
                  <v-list-item-content>
                    <v-list-item-title v-text="dialog.text"></v-list-item-title>
                    <v-list-item-subtitle v-text="dialog.time"></v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
              </v-list-item-group>
            </v-list>
          </v-card>
        </v-col>
      </v-row>
      <v-row no-gutters>
        <v-col v-for="n in 1" :key="n" cols="12" sm="12">
          <v-text-field
            :append-icon-cb="() => {}"
            placeholder="Start typing..."
            single-line
            append-icon="mdi-send"
            color="gray"
            hide-details
            v-model="userInputText"
            v-on:keyup.enter="captureUserInput"
          />
        </v-col>
      </v-row>
    </v-container>

    <v-footer :inset="footer.inset" app>
      <span class="px-4">&copy;Created By Mahmood Khordoo, {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
const axios = require("axios");
export default {
  data: () => ({
    drawers: ["Default (no property)", "Permanent", "Temporary"],
    userInputText: "",
    primaryDrawer: {
      model: null,
      type: "default (no property)",
      clipped: false,
      floating: false,
      mini: false
    },
    items2: [
      { picture: "lego/1", text: "Bot" },
      { picture: "men/28", text: "User" }
    ],
    dialogs: [],
    footer: {
      inset: false
    }
  }),
  methods: {
    captureUserInput: function(e) {
      if (e.keyCode === 13) {
        this.updateDialogs("user", this.userInputText);
        this.queryBot();
        this.userInputText = "";
      }
    },
    updateDialogs: function(agent, text) {
      const icon = agent == "user" ? "men/28" : "lego/1";
      this.dialogs.push({
        agnet: agent,
        iconIndex: icon,
        text: text,
        time: new Date().toLocaleTimeString()
      });
    },
    queryBot: function() {
      let self = this;
      axios
        .post("http://localhost:5000/chat", {
          text: this.userInputText,
          genre: "comedy"
        })
        .then(function(response) {
          self.updateDialogs("bot", response.data.reply);
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  }
};
</script>