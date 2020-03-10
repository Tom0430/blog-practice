<template>
  <section class="slug" v-if="post">
    <div class="image-field">
      <img width="500" height="300" :src="image[0].fields.file.url" class="main-image"/>
    </div>
    <h1 class="slug_title">
       {{ post[0].fields.title }}
    </h1>
    <p class="slug_datetime">{{ slicedDateTime }}</p>
    <p class="slug_tag">タグ：{{ post[0].fields.tag }}</p>
    <div>
      {{ post[0].fields.body.content[0].content[0].value }}
    </div>
  </section>
</template>
<script>
import { createClient } from '~/plugins/contentful.js'
import axios from 'axios';

const client = createClient()
export default {
  data(){
    return{
      post: null,
      image: null
    }
  },
  created() {
    this.$axios
      .get('https://cdn.contentful.com/spaces/' + process.env.CTF_SPACE_ID + '/environments/master/entries'
        , {params: {access_token: process.env.CTF_CDA_ACCESS_TOKEN}})
      .then(res => {
        const title = this.$route.params.slug
        const id = this.$route.params.sys
        this.post = res.data.items.filter((item)=>{
          return item.fields.title === title
        })
        this.image = res.data.includes.Asset.filter((asset) =>{
          return asset.sys.id === this.post[0].fields.images.sys.id
        })
      })
  },
  computed:{
    slicedDateTime: function(){
      let date = this.post[0].sys.createdAt.slice(0,10);
      let time = this.post[0].sys.createdAt.slice(11,16);
      return date + ' ' + time
    },
  }
};
</script>
<style scoped>
.main-image{
  margin: 0,auto;
}

</style>