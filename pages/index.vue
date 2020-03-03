<template>
  <section class="index">
    <card
      v-for="post in displayPosts"
      :key="post.index"
      :title="post.fields.title"
      :image="post.fields.images.fields.file.url"
      :id="post.sys.id"
      :date="post.sys.updatedAt"
    />
    <v-pagination
      v-model="page"
      :length= length
      @input = "pageChange"
    ></v-pagination>
  </section>
</template>

<script>
import Card from '~/components/card.vue'
import { createClient } from '~/plugins/contentful.js'

const client = createClient()
export default {
  data () {
    return {
      posts: [],
      page: 1,
      displayPosts: [],
      pageSize: 3,
      length:null
    }
  },
  transition: 'slide-left',
  components: {
    Card
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
          posts: entries.items
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