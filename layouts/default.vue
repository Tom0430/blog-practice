<template>
  <v-app base>
    <Loading v-if="loading"></Loading>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
      color="primary"
    >
      <v-toolbar-title v-text="title" />
    </v-app-bar>
    <v-content>
      <v-container>
        <v-row class="nuxt-wrapper">
            <nuxt />
        </v-row>
      </v-container>
    </v-content>
    <v-footer
      :fixed="fixed"
      app
      color="primary"
    >
      <span>&copy; 2020</span>
    </v-footer>
  </v-app>
</template>

<script>
import Loading from "@/components/Loading";
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      loading: true,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'BlogSample'
    }
  },
  components:{
    Loading
  },
  methods:{
    finishLoading () {
      this.loading = false
    }
  },
  mounted: function(){
    setTimeout(this.finishLoading, 600)
  }
}
</script>
<style scoped>
.nuxt-wrapper{
  justify-content: center;
}
</style>
