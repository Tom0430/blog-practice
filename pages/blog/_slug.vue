<template>
  <section class="slug">
    <!-- <div v-if="article.fields.images.fields.file">
      <img width="1100" height="800" :src="article.fields.images.fields.file.url"/>
    </div>
    <h1 class="slug_title">
      {{ article.fields.title }}
    </h1>
    <p class="slug_tag">タグ：{{ article.fields.tag }}</p>
    <p class="slug_date">{{ article.fields.dateTime }}</p>
    <div>
      {{ article.fields.body.content[0].content[0].value }}
    </div> -->
    <p>{{$route.params.slug}}</p>
  </section>
</template>
<script>
import { createClient } from '~/plugins/contentful.js'

const client = createClient()
export default {
  data(){
    return{
      article: null
    }
  },
  created() {
    this.$axios
      .get('https://cdn.contentful.com/spaces/' + process.env.CTF_SPACE_ID + '/environments/master/entries'
          , {params: {access_token: process.env.CTF_CDA_ACCESS_TOKEN}})
      .then(res => {
        console.log(res)
        // this.article = res.data;
      });

  }

};
</script>