<template> 
    <v-app>
      <v-navigation-drawer
        v-model="drawer"
        app
        clipped
        permanent
      >
        <v-list dense>
          <v-list-item link to="/welcome">
            <v-list-item-action>
              <v-icon>mdi-help-box</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>Welcome</v-list-item-title>
            </v-list-item-content>
          </v-list-item>

          <v-list-item link to="/wizard">
            <v-list-item-action>
              <v-icon>mdi-wizard-hat</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>Install EdgeTX</v-list-item-title>
            </v-list-item-content>
          </v-list-item>

          <v-list-item v-if="showFlashAdvanced" link to="/flash">
            <v-list-item-action>
              <v-icon>mdi-code-array</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>Advanced Flash</v-list-item-title>
            </v-list-item-content>
          </v-list-item>

          <v-list-item link to="/sdcard">
            <v-list-item-action>
              <v-icon>mdi-database-arrow-right</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>Setup SD Card</v-list-item-title>
            </v-list-item-content>
          </v-list-item>

          <!--<v-list-item link to="/config">
            <v-list-item-action>
              <v-icon>mdi-note-multiple</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>Configure</v-list-item-title>
            </v-list-item-content>
          </v-list-item>-->

          <v-list-item link to="/settings">
            <v-list-item-action>
              <v-icon>mdi-cog</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>Settings</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>
  
      <v-app-bar
        app
        clipped-left
      >
        <v-toolbar-title>EdgeTX Flasher</v-toolbar-title>
      </v-app-bar>
  
      <v-main> 
          <router-view></router-view>
      </v-main>
  
      <v-footer app>
        <span>&copy; {{ new Date().getFullYear() }} Ari Stehney, EdgeTX Team {{ getGitString() }}</span>
      </v-footer>
    </v-app>
</template>

<style>
  body {
    overflow-x: hidden;
  }

  ::-webkit-scrollbar {
    display: none;
  }
</style>

<script>
const fs = require('fs');
const path = require('path');
const {remote} = require("electron");

export default {
  name: 'DefaultFrame',

  data: () => ({
    gitcommit: "",
    showFlashAdvanced: false,
    drawer: true,
  }),

  methods: {
    getGitString() {
      return (this.gitcommit == "") ? "" : this.gitcommit;
    }
  },

  created: function () {
    var self = this;
    var rfn = path.join(path.dirname(remote.app.getAppPath()), "../src/support/")
    
    console.log("Version tag path: "+rfn);
    fs.readFile(path.join(rfn, "git_commit"), 'utf8' , (err, data) => {
      if (err) {
        console.log("Version tag error! "+err)
        return
      }
      self.gitcommit = data;
    });

    this.showFlashAdvanced = this.$store.getters.getOptions.advancedFlash;
  }
};
</script>
