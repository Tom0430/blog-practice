<template>
  <section class="slug">
    <div v-if="post.fields.images.fields.file">
      <img width="1100" height="800" :src="post.fields.images.fields.file.url"/>
    </div>
    <h1 class="slug_title">
      {{ post.fields.title }}
    </h1>
    <p class="slug_tag">タグ：{{ post.fields.tag }}</p>
    <p class="slug_date">{{ post.fields.dateTime }}</p>
    <div>
      {{ post.fields.body.content[0].content[0].value }}
    </div>
  </section>
</template>
<script>
import { createClient } from '~/plugins/contentful.js'

const client = createClient()
export default {
  props: {
    id: {
      type: String,
      default: ''
    }
  },
  transition: 'slide-right',
  async asyncData({ env, params }) {
    return await client
      .getEntry(params.sys)
      .then(entrie => {
        return {
          post: entrie
        }
      })
      .catch(console.error)
  }
}
</script>