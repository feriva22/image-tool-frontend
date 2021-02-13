<template>
  <v-app>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list rounded>
        <v-list-item to="/" router exact>
          <v-list-item-action>
            <v-icon>mdi-home</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Home</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-group prepend-icon="mdi-apps" :value="expand_feature">
          <template v-slot:activator>
            <v-list-item-content>
              <v-list-item-title>Feature</v-list-item-title>
            </v-list-item-content>
          </template>
          <v-list-item
            v-for="(feature, i) in features"
            :key="i"
            :to="feature.to"
            :disabled="!feature.ready"
            router
            exact
          >
            <v-list-item-content>
              <v-list-item-title v-text="feature.title" />
            </v-list-item-content>
            <v-list-item-action>
              <v-icon v-text="feature.icon"></v-icon>
            </v-list-item-action>
          </v-list-item>
        </v-list-group>
        <v-list-item to="/gallery" router exact>
          <v-list-item-action>
            <v-icon>mdi-image-multiple</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Gallery</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item to="/about" router exact>
          <v-list-item-action>
            <v-icon>mdi-chart-bubble</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>About</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title v-text="title" />
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer fixed app>
      <span>&copy; {{ new Date().getFullYear() }} </span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      expand_feature: false,
      clipped: true,
      drawer: true,
      fixed: false,
      features: [
        {
          icon: 'mdi-cloud-upload',
          title: 'Host Image',
          to: '/feature/hosting',
          ready: true,
        },
        {
          icon: 'mdi-crop',
          title: 'Crop Image',
          to: '/feature/crop',
          ready: true,
        },
        {
          icon: 'mdi-arrow-split-horizontal',
          title: 'Resize Image',
          to: '/feature/resize',
          ready: true,
        },
        {
          icon: 'mdi-ceiling-light',
          title: 'Contras Image',
          to: '/feature/contras',
          ready: true,
        },
        {
          icon: 'mdi-swap-horizontal-bold',
          title: 'Convert Image',
          to: '/feature/convert',
          ready: true,
        },
        {
          icon: 'mdi-auto-fix',
          title: 'Restor Image',
          to: '/feature/restoration',
          ready: false,
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Image Tool',
    }
  },
  watch: {
    '$route.path'(value) {
      this.expand_feature = value.includes('/feature/')
    },
  },
}
</script>
