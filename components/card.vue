<template>
    <v-card
      class="mx-auto text-center elevation-2 pa-12 headline"
      max-width="900"
      max-height="900"
    >
      <div @click="toBlogSlug">
        <v-img
          class="white--text align-end"
          :src="image"
          v-ripple
        >
        </v-img>
        <v-card-title>
          <p class="display-1 text--primary">{{ title }}</p>
        </v-card-title>
      </div>

      <v-card-actions>
          <v-chip
            class="ma-2"
            color="pink"
            label
            text-color="white"
            @click="toTagsSearchResult"
          >
            <v-icon left>mdi-label</v-icon>
            {{ tag }}
          </v-chip>
      </v-card-actions>

    </v-card>
</template>
<script>
export default {
  props: {
    title: {
      type: String,
      default: ''
    },
    id: {
      type: String,
      default: ''
    },
    date: {
      type: String,
      default: ''
    },
    image:{
      type: String,
      default: ''
    },
    tag:{
      type: String,
      default: ''
    }
  },
  methods:{
    toTagsSearchResult(){
      this.$router.push({
        name: 'tags-slug',
        params: {
          sys: this.id,
          slug: this.tag
        }
      })
    },
    toBlogSlug() {
      this.$router.push({
        name: 'blog-slug',
        params: {
          sys: this.id,
          slug: this.title
          // ここのsysはnuxt-linkやrouter-linkでも書いてあるので必要な設定
        }
      });
    }
  }
}
</script>

<style scoped>
  .v-application p{
    margin-bottom: 0;
  }
</style>