<template>
  <section class="slug" v-if="post">
    <div>
      <img width="1100" height="800" :src="image[0].fields.file.url"/>
    </div>
    <h1 class="slug_title">
       {{ post[0].fields.title }}
    </h1>
    <p class="slug_tag">タグ：{{ post[0].fields.tag }}</p>
    <p class="slug_date">{{ post[0].fields.dateTime }}</p>
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

        this.post = res.data.items.filter((item)=>{
          let itemEncodedTitle = encodeURI(item.fields.title)
          return itemEncodedTitle === title
        })

        this.image = res.data.includes.Asset.filter((asset) =>{
          return asset.sys.id === this.post[0].fields.images.sys.id
        })
      })
  }
};
</script>