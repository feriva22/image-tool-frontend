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
          Start Change Contras your image
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
                  <v-img
                    max-height="200"
                    max-width="300"
                    accept="image/*"
                    :src="preImgSrc"
                  ></v-img>
                </v-col>
                <v-col cols="6" v-show="result != null">
                  Result Image :
                  <img id="resultImage" style="max-height: 200px; max-width: 300px;" v-bind:src="result" />
                  
                </v-col>
              </v-row>
            </v-col>
            <v-col cols="4">
              <v-row>
                <v-col cols="12">
                  <v-checkbox
                    v-model="usePresetContras"
                    :label="`Use Preset Contras`"
                  ></v-checkbox>
                </v-col>
                <v-col>
                  <v-select
                    v-if="usePresetContras"
                    v-model="targetPresetContras"
                    :disabled="preImgSrc == null"
                    :items="presetsContras"
                    item-text="label"
                    item-value="value"
                    label="Preset Contras"
                    v-on:change="pickPresetContras"
                  ></v-select>
                  <template v-else>
                    <v-slider
                      v-for="(item, i) in manualSetContras"
                      :key="i"
                      :disabled="preImgSrc == null"
                      v-model="item.val"
                      :color="item.color"
                      :label="item.label"
                      ticks
                      thumb-label
                      max="100"
                      min="-100"
                    ></v-slider>
                  </template>
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
      presetsContras: [
        { value: 'vintage', label: 'Vintage' },
        { value: 'lomo', label: 'Lomo' },
        { value: 'clarity', label: 'Clarity' },
        { value: 'sinCity', label: 'Sin City' },
        { value: 'sunrise', label: 'Sunrise' },
        { value: 'crossProcess', label: 'Cross Process' },
        { value: 'orangePeel', label: 'Orange Peel' },
        { value: 'love', label: 'Love' },
        { value: 'grungy', label: 'Grungy' },
        { value: 'jarques', label: 'Jarques' },
        { value: 'pinhole', label: 'Pinhole' },
        { value: 'oldBoot', label: 'Old Boot' },
        { value: 'glowingSun', label: 'Glowing Sun' },
        { value: 'hazyDays', label: 'Hazy Days' },
        { value: 'herMajesty', label: 'Her Majesty' },
        { value: 'nostalgia', label: 'Nostalgia' },
        { value: 'hemingway', label: 'Hemingway' },
        { value: 'concentrate', label: 'Concentrate' },
      ],
      manualSetContras: [
        {
          type: 'contrast',
          label: 'Contrast',
          val: 0,
          color: 'red',
        },
        {
          type: 'brightness',
          label: 'Brightness',
          val: 0,
          color: 'red',
        },
      ],
      usePresetContras: false,
      targetPresetContras: null,
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
  watch: {
    manualSetContras: {
      handler: function(val, oldVal){
        window.Caman('#resultImage',function(){
          var that = this;
          this.revert();
          val.forEach((el) => that[el.type](el.val));
          this.render();
        })
      },
      deep: true
    },
    usePresetContras: function(val){
      if(this.result != null){
      this.targetPresetContras = null
      window.Caman('#resultImage',function(){
                this.revert();
                this.render();
            })
      }
      
    }
  },
  methods: {
    clearImage(ev) {
      this.preImgSrc = null
      this.preImgType = null
      this.targetType = null
      this.result = null
      this.targetPresetContras = null
      this.resetManualContras()
    },
    selectImage(file) {
      if (file !== null) {
        const reader = new FileReader()
        reader.readAsDataURL(file)
        reader.onload = (e) => {
          this.preImgSrc = e.target.result
          this.result = this.preImgSrc
          this.preImgType = this.preImgSrc.substring(
            'data:image/'.length,
            this.preImgSrc.indexOf(';base64')
          )
          //load caman 
          window.Caman('#resultImage',function(){
            this['brightness'](0);
            this['contrast'](0);
            this.render();
          })
        }
      }
    },
    pickPresetContras(ev) {
      console.log(ev)
      window.Caman('#resultImage',function(){
          this.revert();
          this[ev]();
          this.render();
        })
    },
    resetManualContras() {
      this.manualSetContras.forEach((val) => (val.val = 0))
    },
    downloadConvert() {
      window.Caman('#resultImage',function(){
            this.render(function(){
                var base64 = this.toBase64()
                download(base64, 'image.png', 'image/png')
            });
          })
    },
  },
}
</script>
