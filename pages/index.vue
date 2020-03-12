<template>
  <section class="index">
    <div>
      <topImage></topImage>
        <hooper>
          <slide>
            slide 1
          </slide>
          <slide>
            slide 2
          </slide>
        </hooper>
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
    </div>
  </section>
</template>

<script>
import { Hooper, Slide } from 'hooper'
import Card from '~/components/card.vue'
import topImage from '~/components/topImage.vue'
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
    Card,
    topImage,
    Hooper,
    Slide
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