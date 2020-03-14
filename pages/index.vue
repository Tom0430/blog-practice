<template>
  <section class="index">
      <topImage></topImage>
        <card
          v-for="post in displayPosts"
          :key="post.index"
          :title="post.fields.title"
          :tag="post.fields.tag"
          :body="post.fields.body.content[0].content[0].value"
          :image="post.fields.images.fields.file.url"
          :id="post.sys.id"
          :dateTime="post.sys.createdAt"
        />
        <v-pagination
          v-model="page"
          :length= length
          @input = "pageChange"
        ></v-pagination>

  </section>
</template>

<script>
import Card from '~/components/Card.vue'
import TopImage from '~/components/TopImage.vue'
import { createClient } from '~/plugins/contentful.js'
const client = createClient()

export default {
  data () {
    return {
      posts: [],
      images: [],
      page: 1,
      displayPosts: [],
      pageSize: 3,
      length:null,

    }
  },
  transition: 'slide-left',
  components: {
    Card,
    TopImage,
  },
  methods: {
    pageChange: function(pageNumber){
      this.displayPosts = this.posts.slice(this.pageSize*(pageNumber -1), this.pageSize*(pageNumber));
    },
  },
  asyncData({ env, params }) {
    return client
      .getEntries(env.CTF_BLOG_POST_TYPE_ID)
      .then(entries => {
        return {
          posts: entries.items,
          images: entries.includes.Asset
        }
      })
      .catch(console.error)
  },
  mounted: function(){
    this.displayPosts = this.posts.slice(0,this.pageSize);
    this.length = Math.ceil(this.posts.length/this.pageSize);
  }
}
</script>