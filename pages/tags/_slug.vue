<template>
    <div>
        <p>タグ"{{ $route.params.slug }}" の検索結果</p>
        <p>{{ posts }}</p>
        <!-- <p>{{ posts[0] }}</p> -->
        <!-- <card
        v-for="post in posts"
        :key="post.index"
        :title="post.fields.title"
        :tag="post.fields.tag"
        :image="post.fields.images.sys.id"
        :id="post.sys.id"
        :date="post.sys.updatedAt"
        /> -->

    </div>
</template>

<script>
import Card from '~/components/card.vue'
import { createClient } from '~/plugins/contentful.js'
const client = createClient()

export default {
  data(){
    return{
      posts: []
    }
  },
  components: {
    Card
  },
  created() {
    this.$axios
      .get('https://cdn.contentful.com/spaces/' + process.env.CTF_SPACE_ID + '/environments/master/entries'
        , {params: {access_token: process.env.CTF_CDA_ACCESS_TOKEN}})
      .then(res => {
        console.log(res.data)
        const tag = this.$route.params.slug
        this.posts = res.data.items.filter(function(item){
          return item.fields.tag === tag
        })
      })

    }
  }
</script>