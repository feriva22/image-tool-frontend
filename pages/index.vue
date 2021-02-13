<template>
  <div>
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
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <h2>List Feature</h2>
      </v-col>
    </v-row>
    <v-container fluid grid-list-md>
      <v-layout row wrap>
        <v-flex d-flex xs12 sm6 md4>
          <v-hover v-slot="{ hover }">
            <v-card
              :elevation="hover ? 20 : 2"
              :class="{ 'on-hover': hover }"
              color="purple"
              to="/feature/hosting"
              dark
            >
              <v-card-title primary class="title"
                ><v-icon class="margin-icon">mdi-cloud-upload</v-icon> Host
                Image</v-card-title
              >
              <v-card-text>{{ desc.hostimg }}</v-card-text>
            </v-card>
          </v-hover>
        </v-flex>
        <v-flex d-flex xs12 sm6 md3>
          <v-layout row wrap>
            <v-flex d-flex>
              <v-hover v-slot="{ hover }">
                <v-card
                  :elevation="hover ? 20 : 2"
                  :class="{ 'on-hover': hover }"
                  color="indigo"
                  to="/feature/convert"
                  dark
                >
                  <v-card-title primary class="title"
                    ><v-icon class="margin-icon"
                      >mdi-swap-horizontal-bold</v-icon
                    >Convert Image</v-card-title
                  >
                  <v-card-text>{{ desc.convert }}</v-card-text>
                </v-card>
              </v-hover>
            </v-flex>
            <v-flex d-flex>
              <v-layout row wrap>
                <v-flex d-flex xs12>
                  <v-hover v-slot="{ hover }">
                    <v-card
                      :elevation="hover ? 20 : 2"
                      :class="{ 'on-hover': hover }"
                      color="red "
                      to="/feature/resize"
                      dark
                    >
                      <v-card-title
                        ><v-icon class="margin-icon"
                          >mdi-arrow-split-horizontal</v-icon
                        >Resize Image</v-card-title
                      >
                      <v-card-text>{{ desc.resize }}</v-card-text>
                    </v-card>
                  </v-hover>
                </v-flex>
                <v-flex d-flex xs12>
                  <v-hover v-slot="{ hover }">
                    <v-card
                      :elevation="hover ? 20 : 2"
                      :class="{ 'on-hover': hover }"
                      color="black lighten-2"
                      to="/feature/contras"
                      dark
                    >
                      <v-card-title
                        ><v-icon class="margin-icon">mdi-ceiling-light</v-icon
                        >Contras Image</v-card-title
                      >
                      <v-card-text>{{ desc.contras }}</v-card-text>
                    </v-card>
                  </v-hover>
                </v-flex>
              </v-layout>
            </v-flex>
          </v-layout>
        </v-flex>
        <v-flex d-flex xs12 sm6 md2 child-flex>
          <v-hover v-slot="{ hover }">
            <v-card
              :elevation="hover ? 20 : 2"
              :class="{ 'on-hover': hover }"
              color="green "
              to="/feature/crop"
              dark
            >
              <v-card-title primary class="title"
                ><v-icon class="margin-icon">mdi-crop</v-icon>Crop
                Image</v-card-title
              >
              <v-card-text>{{ desc.crop }}</v-card-text>
            </v-card>
          </v-hover>
        </v-flex>
        <v-flex d-flex xs12 sm6 md3>
          <v-hover v-slot="{ hover }">
            <v-card
              :elevation="hover ? 20 : 2"
              :class="{ 'on-hover': hover }"
              color="blue "
              dark
            >
              <v-card-title primary class="title"
                ><v-icon class="margin-icon">mdi-auto-fix</v-icon>Restore Image
                <b>Comming Soon</b></v-card-title
              >
              <v-card-text>{{ desc.restore }}</v-card-text>
            </v-card>
          </v-hover>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      desc: {
        hostimg:
          'Fitur ini dapat berguna untuk menyimpan gambar anda di internet dengan mudah, tanpa akun berbayar , dan aman dengan hak akses, gunakan secara gratis',
        crop:
          'Fitur ini dapat berguna untuk crop gambar kamu dengan mudah, lalu download',
        resize: 'Fitur untuk meresize gambar sesuai yang kamu inginkan',
        contras:
          'Fitur  untuk memanipulasi kontras gambar, baik manual atau menggunakan preset ',
        convert: 'Fitur untuk konversi type gambar',
        restore:
          'Fitur powerfull menerapkan Deep Learning untuk merestorasi foto rusak menjadi foto baru',
      },
    }
  },
}
</script>

<style scoped>
.margin-icon {
  margin-right: 5px;
}

.v-card {
  transition: opacity 0.4s ease-in-out;
}

.v-card:not(.on-hover) {
  opacity: 0.6;
}
</style>
