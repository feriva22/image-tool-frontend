<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
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
          Start Change Size your image
        </v-card-title>
        <v-card-text>
          <p></p>
          <v-row>
            <v-col cols="8">
              <v-row>
                <v-col cols="12">
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
                <v-col cols="6" v-show="preImgSrc != null">
                  Image want to be process , Detected Type
                  <b>{{ preImgType }}</b> :
                  <img id="preImgSrc" style="max-height:200px; max-width: 200px;" v-bind:src="preImgSrc"/>
                </v-col>
                <v-col cols="6" v-show="preImgSrc != null">
                 Height Image : {{ preHeight }} <br/>
                 Width Image : {{ preWidth }} 
                </v-col>
              </v-row>
            </v-col>
            <v-col cols="4">
              <v-row>
                <v-col cols="12">
                     <v-text-field
                        :disabled="preImgSrc == null"
                        v-model="setWidth"
                        :value="setWidth"
                        label="Width"
                    ></v-text-field>
                </v-col>
                <v-col cols="12">
                     <v-text-field
                        :disabled="preImgSrc == null"
                        v-model="setHeight"
                        :value="setHeight"
                        label="Heigth"
                    ></v-text-field>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn
            v-show="preImgSrc != null"
            :loading="processConvert"
            color="primary"
            @click="downloadConvert"
          >
            Download
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
      preImgSrc: null,
      preImgType: null,
      processConvert: false,
      preHeight: null,
      preWidth: null,
      setHeight: 200,
      setWidth: 200,
      rules: [
        (value) =>
          !value ||
          value.size < 2000000 ||
          'Image size should be less than 2 MB!',
      ],
    }
  },
  watch: {
  },
  methods: {
    clearImage(ev) {
      this.preImgSrc = null
      this.preImgType = null
    },
    selectImage(file) {
      if (file !== null) {
        const reader = new FileReader()
        console.log(file)
        reader.readAsDataURL(file)
        var that = this
        reader.onload = (e,) => {
          var image = new Image()
          image.src = e.target.result
          image.onload = function(){
              that.preHeight = image.height
              that.preWidth = image.width
          }

          this.preImgSrc = e.target.result
          this.preImgType = this.preImgSrc.substring(
            'data:image/'.length,
            this.preImgSrc.indexOf(';base64')
          )
          
        }
      }
    },
    downloadConvert() {
    var that = this
      window.Caman('#preImgSrc',function(){
            console.log(that.setHeight)
            this.resize({width: that.setWidth, height: that.setHeight});
            this.render(function(){
                var base64 = this.toBase64()
                download(base64, 'image.png', 'image/png')
            });
          })
    },
  },
}
</script>
