<template>
  <v-row>
    <v-col class="text-center" md="12">
      <h1>Gallery Public Image</h1>
    </v-col>
    <v-col>
      <v-container fluid>
        <v-row dense>
          <v-col v-for="(item, key) in listImg" :key="key" md="3" sm="12">
            <v-card>
              <v-img
                :src="item.url"
                height="100px"
                class="white--text align-end"
              >
              </v-img>
              <v-card-title
                ><v-icon>mdi-account</v-icon> {{ item.title }}
              </v-card-title>
              <v-card-subtitle>
                {{ item.description }}
                <br />
                <v-icon>mdi-clock</v-icon>
                {{ $moment(item.created_date).fromNow() }}</v-card-subtitle
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" :href="item.gen_url" icon>
                  <v-icon>mdi-eye</v-icon>
                </v-btn>
                <v-btn color="success" :href="item.url" icon>
                  <v-icon>mdi-download</v-icon>
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      listImg: [],
    }
  },
  mounted() {
    this.getGalleryData()
  },
  methods: {
    async getGalleryData() {
      try {
        const imgData = await this.$axios.$get('/api/gallery')
        this.listImg = imgData
        console.log(imgData)
      } catch (e) {
        console.log(e)
      }
    },
  },
}
</script>
