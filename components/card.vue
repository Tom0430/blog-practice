<template>
    <v-card
      class="mx-auto text-center elevation-2 pa-12 headline"
      max-width="900"
      max-height="900"
      v-ripple
    >
        <nuxt-link
        :to="{ name: 'blog-slug', params: {
        sys: id
        }}"
        class="wrapper"
        >
            <v-img
                class="white--text align-end"
                :src="images"
            >
                <v-card-title>{{ title }}</v-card-title>
            </v-img>

            <v-card-text class="text--primary">
            </v-card-text>

            <v-card-actions>
                <v-btn
                color="orange"
                text
                >
                この記事を読む
                </v-btn>
            </v-card-actions>
        </nuxt-link>
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
    images:{
        type: String,
        default: ''
    }
  },
}
</script>