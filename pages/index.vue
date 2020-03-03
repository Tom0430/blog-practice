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
      :length="5"
      @input = "getNumber"
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
      page: 1,
      displayPosts: [],
      pageSize: 3,
    }
  },
  transition: 'slide-left',
  components: {
    Card
  },
  methods: {
    getNumber: function(number){
      console.log(number)
    }
  },
  asyncData({ env, params }) {
    return client
      .getEntries(env.CTF_BLOG_POST_TYPE_ID)
      .then(entries => {
        return {
          posts: entries.items.slice(0,2)
        }
      })
      .catch(console.error)
  },
  mounted: function(){
    this.displayPosts = this.posts.slice(0,this.pageSize);

  }
}
</script>