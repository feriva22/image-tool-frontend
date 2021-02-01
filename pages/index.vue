<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="12">
      <v-row>
        <v-col>
          <v-img
            src="/logo.png"
            max-height="250"
            max-width="200"
            alt="Logo"
            class="ml-auto mr-auto"
          ></v-img>
        </v-col>
      </v-row>
      <v-card>
        <v-card-title class="headline">
          Start Restore your old image
        </v-card-title>
        <v-card-text>
          <p>Free to use restoration old image with Deep learning</p>
          <v-file-input
            :rules="rules"
            ref="photoField"
            :disabled="processConvert"
            accept="image/png, image/jpeg, image/bmp"
            placeholder=""
            prepend-icon="mdi-camera"
            label="Choose image"
            @change="selectImage"
            @click:clear="clearImage"
          ></v-file-input>
          <v-row>
            <v-col v-show="hasImage">
              Image want to be process :
              <v-img
                max-height="500"
                max-width="600"
                accept="image/*"
                :src="preImgSrc"
              ></v-img>
            </v-col>
          </v-row>
          <v-row>
            <v-col v-show="postImgSrc != null">
              Result image :
              <v-img
                max-height="500"
                max-width="600"
                accept="image/*"
                :src="postImgSrc"
              >
              </v-img>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn
            v-show="hasImage && postImgSrc == null"
            :loading="processConvert"
            color="primary"
            @click="convertImg"
          >
            Start Convert
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      hasImage: false,
      preImgSrc: null,
      postImgSrc: null,
      processConvert: false,
      rules: [
        (value) =>
          !value ||
          value.size < 2000000 ||
          'Image size should be less than 2 MB!',
      ],
    }
  },
  methods: {
    clearImage(ev) {
      this.hasImage = false
      this.preImgSrc = null
      this.postImgSrc = null
    },
    selectImage(file) {
      if (file !== null) {
        const reader = new FileReader()
        reader.readAsDataURL(file)
        reader.onload = (e) => {
          this.preImgSrc = e.target.result
          console.log(this.preImgSrc)
          this.hasImage = true
        }
      }
    },
    async convertImg() {
      this.processConvert = true
      let dataBase64 = this.preImgSrc.split(',')[1]
      const typeImg = this.preImgSrc.substring(
        'data:image/'.length,
        this.preImgSrc.indexOf(';base64')
      )
      try {
        const resp = await this.$axios.$post('/api/restoration/convert', {
          type: typeImg,
          img: dataBase64,
        })
        this.postImgSrc = resp.postImg
      } catch (e) {
        console.log(e)
      }
      this.processConvert = false
    },
  },
}
</script>
