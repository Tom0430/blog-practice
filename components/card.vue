<template>
    <v-card
      class="mx-auto text-center elevation-2 pa-12 headline"
      max-width="900"
      max-height="900"
    >
      <span>{{ slicedDateTime }}</span>
      <div @click="toBlogSlug">
        <v-img
          class="white--text align-end"
          :src="image"
          v-ripple
        >
        </v-img>
        <v-card-title>
          <p class="display-1 text--primary">{{ title | truncate }}</p>
        </v-card-title>
      </div>

      <v-card-actions>
          <v-chip
            class="ma-2"
            color="pink"
            label
            text-color="white"
            @click="sameTagList"
          >
            <v-icon left>mdi-label</v-icon>
            {{ tag }}
          </v-chip>
      </v-card-actions>

    </v-card>
</template>
<script>
export default {
  filters: {
    truncate: function(value) {
      var length = 20;
      var omission = "...";
      if (value.length <= length) {
        return value;
      }
      return value.substring(0, length) + omission;
    }
  },
  props: {
    title: {
      type: String,
      default: ''
    },
    id: {
      type: String,
      default: ''
    },
    dateTime: {
      type: String,
      default: ''
    },
    image:{
      type: String,
      default: ''
    },
    body:{
      type: String,
      default: ''
    },
    tag:{
      type: String,
      default: ''
    }
  },
  computed:{
    slicedDateTime: function(){
      let date = this.dateTime.slice(0,10);
      let time = this.dateTime.slice(11,16);
      return date + ' ' + time
    },
  },
  methods:{
    sameTagList(){
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