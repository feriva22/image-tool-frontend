<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <template v-if="isLoadData"> Loading data.... </template>
      <template v-else-if="notValid">Invalid URL</template>
      <template v-else-if="isAuthorized">
        <v-row justify="center" align="center">
          <v-col cols="6">
            <v-img
              max-height="500"
              max-width="600"
              accept="image/*"
              :src="dataResp.url"
            ></v-img>
          </v-col>
        </v-row>
        <v-row justify="center" align="center">
          <v-col cols="6">
            <v-card>
              <v-card-title class="headline">{{ dataResp.title }}</v-card-title>
              <v-card-subtitle>{{ dataResp.description }}</v-card-subtitle>
              <v-card-text>
                <v-row>
                  <v-col cols="2">
                    <v-icon>mdi-eye</v-icon> {{ dataResp.view }}
                  </v-col>
                  <v-col cols="2">
                    <v-icon>mdi-thumb-up-outline</v-icon> {{ dataResp.like }}
                  </v-col>
                </v-row>
                <v-row>
                  <v-col>
                    Created {{ $moment(dataResp.created_date).fromNow() }}
                  </v-col>
                </v-row>
              </v-card-text>
              <v-card-actions>
                <template v-if="dataResp.permission == 'PUBLIC'">
                  Share:
                  <div style="margin-left: 10px"></div>

                  <ShareNetwork
                    network="facebook"
                    class="no-decoration"
                    :url="currUrl"
                    :title="dataResp.title"
                    :description="dataResp.description"
                    :quote="'Share my image ' + dataResp.title"
                    hashtags="imagetool.tech, hosting"
                  >
                    <v-icon color="blue">mdi-facebook</v-icon>
                  </ShareNetwork>
                  <div style="margin-left: 10px"></div>
                  <ShareNetwork
                    network="twitter"
                    class="no-decoration"
                    :url="currUrl"
                    :title="dataResp.title"
                    :description="dataResp.description"
                    :quote="'Share my image ' + dataResp.title"
                    hashtags="imagetool.tech, hosting"
                  >
                    <v-icon color="blue">mdi-twitter</v-icon>
                  </ShareNetwork>
                  <div style="margin-left: 10px"></div>
                  <ShareNetwork
                    network="whatsapp"
                    class="no-decoration"
                    :url="currUrl"
                    :title="'Share my image ' + dataResp.title"
                    :description="dataResp.description"
                    :quote="'Share my image ' + dataResp.title"
                    hashtags="imagetool.tech, hosting"
                  >
                    <v-icon color="green">mdi-whatsapp</v-icon>
                  </ShareNetwork>
                </template>
                <v-spacer></v-spacer>
                <v-btn color="success" :href="dataResp.url" icon>
                  <v-icon>mdi-download</v-icon>
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </template>
      <template v-else>
        Input password to access image
        <v-row>
          <v-col cols="2">
            <v-form ref="passwordForm" v-model="valid" lazy-validation>
              <v-text-field
                v-model="passwordAccess.value"
                :value="passwordAccess.value"
                :error-messages="errorMessage"
                :rules="[rules.required]"
                label="Password"
                required
              ></v-text-field>
            </v-form>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="4">
            <v-btn :disabled="!valid" elevation="2" @click="submitPassword"
              >Submit</v-btn
            >
          </v-col>
        </v-row>
      </template>
    </v-col>
  </v-row>
</template>

<script>
export default {
  async asyncData({ params }) {
    const id = params.id
    return { id }
  },
  data() {
    return {
      rules: {
        required: (value) => !!value || 'Required',
      },
      currUrl: window.location.href,
      isLoadData: true,
      valid: true,
      isAuthorized: false,
      passwordAccess: {
        value: '',
        error: '',
      },
      errorMessage: '',
      dataResp: null,
      notValid: false,
    }
  },
  async beforeMount() {
    try {
      const resp = await this.$axios.$post(
        '/api/image-tool/hosting/validate/' + this.id
      )
      if (resp.permission == 'SECURE') {
        this.isAuthorized = false
      } else {
        this.isAuthorized = true
        this.dataResp = resp
      }
      this.identifier = resp
      console.log(resp)
    } catch (e) {
      this.isLoadData = false
      this.notValid = true
    }
    this.isLoadData = false
  },
  methods: {
    async submitPassword(e) {
      if (this.$refs.passwordForm.validate()) {
        try {
          if (!this.isAuthorized) {
            const resp = await this.$axios.$post(
              '/api/image-tool/hosting/get_info_secure/' + this.id,
              { secret: this.passwordAccess.value }
            )
            this.isAuthorized = true
            this.dataResp = resp
          }
        } catch (e) {
          this.errorMessage = 'Invalid secret'
          this.$refs.passwordForm.resetValidation()
        }
      }
    },
  },
}
</script>

<style scoped>
.no-decoration {
  text-decoration: none !important;
}
</style>
