
<template>
  <v-app id="sandbox" app dark>
    <v-navigation-drawer
      v-model="primaryDrawer.model"
      class="chatbar--gradient"
      app
      left
      :clipped="false"
      permanent
      mini-variant-width="70"
      width="250px"
    >
      <v-subheader class="ma-0 pd-10 mt-0 gray--text text--darken-1" id="status">Online users</v-subheader>
      <v-list dense>
        <v-list>
          <v-list-item v-for="user in users" :key="user.name" link>
            <v-list-item-avatar>
              <img :src="`https://randomuser.me/api/portraits/${user.picture}.jpg`" alt />
            </v-list-item-avatar>
            <v-list-item-title v-text="user.name" />
          </v-list-item>
        </v-list>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar :clipped-left="primaryDrawer.clipped" app color="cyan">
      <v-app-bar-nav-icon
        v-if="primaryDrawer.type !== 'permanent'"
        @click.stop="primaryDrawer.model = !primaryDrawer.model"
      />
      <v-icon class="mx-4" large>mdi-robot</v-icon>
      <v-toolbar-title>Chatbot</v-toolbar-title>
    </v-app-bar>
    <v-container class="grey lighten-5" fluid>
      <v-row no-gutters>
        <v-col v-for="n in 1" :key="n" cols="12" sm="12">
          <v-card class="pa-2" outlined tile height="700">
            <v-list style="max-height: 650px" height="650px" id="chatbox" class="overflow-y-auto">
              <v-subheader>CONVERSATION</v-subheader>
              <v-list-item-group color="primary">
                <v-list-item v-for="(dialog,i) in dialogs" :key="i" id="chat-item">
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
        <v-col cols="12" sm="12">
          <v-text-field
            :append-icon-cb="() => {}"
            placeholder="Say something..."
            single-line
            append-icon="mdi-send"
            hide-details
            color="primary"
            v-model="userInputText"
            v-on:keyup.enter="captureUserInput"
          >
            <v-icon slot="append" color="primary">mdi-send</v-icon>
          </v-text-field>
        </v-col>
      </v-row>
    </v-container>

    <v-footer
      :inset="footer.inset"
      app
      color="#00acc1"
      class="white--text font-italic font-weight-light overline"
    >
      <span class="px-2 white--text">
        &copy;{{ new Date().getFullYear()}} Created by
        <a href="http://www.github.com/khordoo">
          <span class="white--text">Mahmood Khordoo</span>
        </a>
      </span>
    </v-footer>
  </v-app>
</template>

<script>
const NLP_SERVER = "http://localhost:5000/chat";
const axios = require("axios");
export default {
  data: () => ({
    drawers: ["Default (no property)", "Permanent", "Temporary"],
    userInputText: "",
    offsetTop: 0,
    primaryDrawer: {
      model: null,
      type: "default (no property)",
      clipped: false,
      floating: false,
      mini: false
    },
    users: [
      { picture: "lego/1", name: "Bot" },
      { picture: "men/85", name: "User" }
    ],
    dialogs: [],
    footer: {
      inset: false
    }
  }),
  methods: {
    captureUserInput(e) {
      if (e.keyCode === 13) {
        this.updateDialogs("user", this.userInputText);
        this.queryBot();
        this.userInputText = "";
      }
    },
    updateDialogs(agent, text) {
      const icon = agent == "bot" ? "lego/1" : "men/85";
      this.dialogs.push({
        agnet: agent,
        iconIndex: icon,
        text: text,
        time: new Date().toLocaleTimeString()
      });
    },
    queryBot() {
      let self = this;
      axios
        .post(NLP_SERVER, {
          text: this.userInputText
        })
        .then(function(response) {
          self.updateDialogs("bot", response.data.reply);
          self.scrollToEnd();
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    scrollToEnd: function() {
      document.getElementById("chatbox").scrollTop = document.getElementById(
        "chatbox"
      ).scrollHeight;
    }
  }
};
</script>