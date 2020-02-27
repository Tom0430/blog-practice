<template>
    <v-card
      class="mx-auto"
      max-width="400"
    >
        <nuxt-link
        :to="{ name: 'blog-slug', params: {
        sys: id
        }}"
        class="wrapper"
        >
            <v-img
                class="white--text align-end"
                height="200px"
                src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
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
    }
  },
}
</script>