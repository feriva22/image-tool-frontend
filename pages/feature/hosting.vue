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
        <v-card-title class="headline"> Start Host your image </v-card-title>
        <v-card-text>
          <p></p>
          <v-row>
            <v-col cols="12" md="8">
              <v-file-input
                :rules="rules"
                ref="photoField"
                :disabled="processUpload"
                accept="image/png, image/jpeg, image/bmp"
                placeholder=""
                prepend-icon="mdi-camera"
                label="Choose image"
                @change="selectImage"
                @click:clear="clearImage"
              ></v-file-input>
              <v-row>
                <v-col v-show="preImgSrc != null">
                  Image want to be process , Detected Type
                  <b>{{ preImgType }}</b> :
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
                  Result Link <a :href="result">{{ result }}</a>
                </v-col>
              </v-row>
            </v-col>
            <v-col cols="12" md="4">
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    :disabled="preImgSrc == null || processUpload"
                    v-model="titleImg"
                    :value="titleImg"
                    label="Title"
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    :disabled="preImgSrc == null || processUpload"
                    v-model="descImg"
                    :value="descImg"
                    label="Description"
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-select
                    v-model="expiryImg"
                    :disabled="preImgSrc == null || processUpload"
                    :items="expiryOption"
                    label="Expiry file"
                  ></v-select>
                </v-col>
                <v-col cols="12">
                  <v-select
                    v-model="permissionType"
                    :disabled="preImgSrc == null || processUpload"
                    :items="supportedType"
                    label="Permission Type"
                  ></v-select>
                </v-col>
                <v-col>
                  <v-text-field
                    v-show="permissionType == 'SECURE'"
                    :disabled="preImgSrc == null || processUpload"
                    v-model="passwordSecurePermission"
                    :value="passwordSecurePermission"
                    label="Password"
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn
            v-show="permissionType != null && preImgSrc != null"
            :loading="processUpload"
            color="primary"
            @click="uploadImg"
          >
            Start Upload
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
      expiryOption: [
        { text: 'No expiry', value: 'P0M' },
        { value: 'P5M', text: 'After 5 minute' },
        { value: 'P15M', text: 'After 15 minute' },
        { value: 'P30M', text: 'After 30 minute' },
        { value: 'P1H', text: 'After 1 hour' },
        { value: 'P2H', text: 'After 2 hour' },
        { value: 'P6H', text: 'After 6 hour' },
        { value: 'P1D', text: 'After 1 day' },
        { value: 'P2D', text: 'After 2 day' },
        { value: 'P3D', text: 'After 3 day' },
        { value: 'P1W', text: 'After 1 week' },
        { value: 'P2W', text: 'After 2 week' },
      ],
      supportedType: ['PUBLIC', 'PRIVATE', 'SECURE'],
      titleImg: null,
      descImg: null,
      expiryImg: null,
      preImgSrc: null,
      preImgType: null,
      permissionType: null,
      passwordSecurePermission: null,
      result: null,
      processUpload: false,
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
      this.permissionType = null
      this.expiryImg = null
      this.descImg = null
      this.titleImg = null
      this.passwordSecurePermission = null
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
    async uploadImg() {
      this.processUpload = true
      let dataBase64 = this.preImgSrc.split(',')[1]
      try {
        const resp = await this.$axios.$post(
          '/api/image-tool/hosting/gen_url',
          {
            title: this.titleImg,
            description: this.descImg,
            img: dataBase64,
            permission: this.permissionType,
            pass_secure: this.passwordSecurePermission,
            upload_expiration: this.expiryImg,
          }
        )
        console.log(resp)
        this.result = resp.gen_url
      } catch (e) {
        console.log(e)
      }
      this.processUpload = false
    },
  },
}
</script>
