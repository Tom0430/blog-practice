<template>
    <div>
        <p>タグ"{{ $route.params.slug }}" の一覧</p>
        <div v-if="posts" class="cards-wrapper">
          <card
          v-for="post in posts"
          :key="post.index"
          :title="post.fields.title"
          :tag="post.fields.tag"
          :id="post.sys.id"
          :date="post.sys.updatedAt"
          :image="post.image[0].fields.file.url"
          />
        </div>
    </div>
</template>

<script>
import Card from '~/components/Card.vue'
import { createClient } from '~/plugins/contentful.js'
const client = createClient()

export default {
  data(){
    return{
      posts: null,
      images: []
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
        const tag = this.$route.params.slug
        this.posts = res.data.items.filter(function(item){
          return item.fields.tag === tag
        })
        this.posts.forEach(post => {
          post.image = res.data.includes.Asset.filter((item)=>{
            return post.fields.images.sys.id === item.sys.id
          })
        });
      })

    }
  }
</script>