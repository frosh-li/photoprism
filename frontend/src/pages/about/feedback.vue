<template>
  <div class="p-page p-page-support">
    <v-toolbar flat color="secondary">
      <v-toolbar-title v-if="sent">
        <translate>Your message has been sent</translate>
      </v-toolbar-title>
      <v-toolbar-title v-else>
        <translate>Contact Us</translate>
      </v-toolbar-title>

      <!-- v-spacer></v-spacer>

      <v-btn icon href="https://github.com/photoprism/photoprism" target="_blank" class="action-github" title="GitHub">
        <img src="/static/brands/github.svg" width="24" alt="GitHub">
      </v-btn -->
    </v-toolbar>
    <v-container fluid class="pa-4" v-if="sent">
      <p class="body-1">We'll get back to you as soon as possible!</p>
    </v-container>
    <v-form autocomplete="off" class="pa-3" ref="form"
            v-model="valid"
            lazy-validation v-else>
      <v-layout row wrap>
        <!-- v-flex xs12 sm6 lg4 xl2 grow class="pa-2">
          <v-text-field :required="true" hide-details
                        v-model="form.Subject" browser-autocomplete="off"
                        :label="$gettext('Subject')"></v-text-field>
        </v-flex -->
        <v-flex xs12 class="pa-2">
          <v-select
              :disabled="busy"
              :items="options.FeedbackCategories()"
              :label="$gettext('Category')"
              color="secondary-dark"
              background-color="secondary-light"
              v-model="form.Category"
              flat solo hide-details required
              browser-autocomplete="off"
              class="input-category"
              :rules="[v => !!v || $gettext('Required')]"
          ></v-select>
        </v-flex>

        <v-flex xs12 class="pa-2">
          <v-textarea required auto-grow flat solo hide-details browser-autocomplete="off"
                      v-model="form.Message" rows="10" :rules="[v => !!v || $gettext('Required')]"
                      :label="$gettext('How can we help?')"></v-textarea>
        </v-flex>

        <v-flex xs12 sm6 class="pa-2">
          <v-text-field flat solo hide-details browser-autocomplete="off"
                        color="secondary-dark"
                        background-color="secondary-light"
                        :label="$gettext('Name')" type="text" v-model="form.UserName">
          </v-text-field>
        </v-flex>

        <v-flex xs12 sm6 class="pa-2">
          <v-text-field flat solo hide-details required browser-autocomplete="off"
                        color="secondary-dark" :rules="[v => !!v || $gettext('Required')]"
                        background-color="secondary-light"
                        :label="$gettext('E-Mail')" type="email" v-model="form.UserEmail">
          </v-text-field>
        </v-flex>

        <v-flex xs12 grow class="px-2 py-1">
          <v-btn color="secondary-dark"
                 class="white--text ml-0"
                 depressed
                 :disabled="!form.Category || !form.Message || !form.UserEmail"
                 @click.stop="send">
            <translate>Send</translate>
            <v-icon right dark>send</v-icon>
          </v-btn>
        </v-flex>

      </v-layout>

    </v-form>

    <p-about-footer></p-about-footer>
  </div>
</template>

<script>
import * as options from "options/options";
import Api from "../../common/api";

export default {
  name: 'p-page-support',
  data() {
    return {
      sent: false,
      busy: false,
      valid: false,
      options: options,
      form: {
        Category: "",
        Message: "",
        UserName: "",
        UserEmail: "",
        UserAgent: navigator.userAgent,
        UserLocales: navigator.language,
      }
    };
  },
  methods: {
    send() {
      if (this.$refs.form.validate()) {
        Api.post("feedback", this.form).then(() => {
          this.$notify.success(this.$gettext("Message sent"));
          this.sent = true;
        });
      } else {
        this.$notify.error(this.$gettext("All fields are required"));
      }

    },
  },
};
</script>
