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
          Start Convert your image to another type
        </v-card-title>
        <v-card-text>
          <p></p>
          <v-row>
            <v-col cols="8">
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
            </v-col>
            <v-col cols="4">
              <v-select
                v-model="targetType"
                :disabled="preImgSrc == null"
                :items="supportedType"
                label="Convert To"
              ></v-select>
            </v-col>
          </v-row>
          <v-row>
            <v-col v-show="preImgSrc != null">
              Image want to be process , Detected Type <b>{{ preImgType }}</b> :
              <v-img
                max-height="500"
                max-width="600"
                accept="image/*"
                :src="preImgSrc"
              ></v-img>
            </v-col>
          </v-row>
          <v-row>
            <v-col v-show="result != null">
              Result Image :
              <v-btn target="_blank" :href="result" color="primary">
                Download
              </v-btn>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn
            v-show="targetType != null && preImgSrc != null && result == null"
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
      supportedType: ['jpg', 'png', 'bmp', 'gif', 'tiff'],
      targetType: null,
      preImgSrc: null,
      preImgType: null,
      result: null,
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
      this.preImgSrc = null
      this.preImgType = null
      this.targetType = null
      this.result = null
    },
    selectImage(file) {
      if (file !== null) {
        const reader = new FileReader()
        reader.readAsDataURL(file)
        reader.onload = (e) => {
          this.preImgSrc = e.target.result
          this.preImgType = this.preImgSrc.substring(
            'data:image/'.length,
            this.preImgSrc.indexOf(';base64')
          )
        }
      }
    },
    async convertImg() {
      this.processConvert = true
      let dataBase64 = this.preImgSrc.split(',')[1]
      try {
        const resp = await this.$axios.$post('/api/image-tool/convert', {
          type: this.preImgType,
          img: dataBase64,
          targetType: this.targetType,
        })
        this.result = resp.postImg
      } catch (e) {
        console.log(e)
      }
      this.processConvert = false
    },
  },
}
</script>
