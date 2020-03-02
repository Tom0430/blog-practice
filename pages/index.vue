<template>
  <section class="index">
    <card
      v-for="(post,i ) in posts"
      :key="i"
      :title="post.fields.title"
      :image="post.fields.images.fields.file.url"
      :id="post.sys.id"
      :date="post.sys.updatedAt"
    />
    <paginate
      :page-count="getPageCount"
      :page-range="3"
      :margin-pages="2"
      :click-handler="clickCallback"
      :prev-text="'＜'"
      :next-text="'＞'"
      :container-class="'pagination'"
      :page-class="'page-item'">
    </paginate>
  </section>
</template>

<script>
import Card from '~/components/card.vue'
import { createClient } from '~/plugins/contentful.js'

var posts = [];
for(var i=1; i < 100; i++){
  posts.push('post-'+i);
}

const client = createClient()
export default {
  transition: 'slide-left',
  data() {
     posts: posts,
     parPage: 3,
     currentPage: 1
  },
  components: {
    Card
  },
  methods: {
    clickCallback: function (pageNum) {
      this.currentPage = Number(pageNum);
    }
  },
  computed: {
     getPosts: function() {
      let current = this.currentPage * this.parPage;
      let start = current - this.parPage;
      return this.posts.slice(start, current);
     },
     getPageCount: function() {
      return Math.ceil(this.posts.length / this.parPage);
     }
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
}
</script>

//テスト
//さらにテスト