<template>
  <section class="slug">
    <!-- <div v-if="article[0].fields.images.fields.file">
      <img width="1100" height="800" :src="article[0].fields.images.fields.file.url"/>
    </div>
    <h1 class="slug_title">
      {{ article[0].fields.title }}
    </h1>
    <p class="slug_tag">タグ：{{ article[0].fields.tag }}</p>
    <p class="slug_date">{{ article[0].fields.dateTime }}</p>
    <div>
      {{ article[0].fields.body.content[0].content[0].value }}
    </div> -->
    <p>

      {{ article }}
    </p>

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
        console.log(res.data.items)
        const title = this.$route.params.slug
        const items = res.data.items
        console.log(items)
        const article = items.filter(function(item){
          console.log(item)
          return item.fields.title === title
        })
        console.log(article)
        this.article = article;
      });
  }

};
</script>