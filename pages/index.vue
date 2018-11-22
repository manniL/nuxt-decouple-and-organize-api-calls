<template>
  <section>
    <main>
      <h1 class="title">Posts</h1>
      <ul>
        <li :key="id" v-for="{ id, title } in slicedPosts">
          <nuxt-link v-if="title !== 'foo'" :to="`/${id}`">
            {{ title }}
          </nuxt-link>
          <p v-else>{{ title }}</p>
        </li>
      </ul>
      <div class="links">
        <button @click="createPost" class="button--grey">
          Create a post (send POST request)
        </button>
      </div>
    </main>
  </section>
</template>

<script>
export default {
  data() {
    return {
      posts: []
    }
  },
  async asyncData(ctx) {
    return {
      posts: await ctx.app.$postRepository.index()
    }
  },
  computed: {
    slicedPosts() {
      return this.posts.slice(-3)
    }
  },
  methods: {
    async createPost() {
      const result = await this.$postRepository.create({
        title: 'foo',
        body: 'bar',
        userId: 1
      })
      console.log(result)
      // Fix ids to be unique
      this.posts.push({ ...result, id: Number(this.posts.slice(-1)[0].id) + 1 })
    }
  }
}
</script>

<style scoped>
.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 400;
  font-size: 100px;
  color: #526488;
  letter-spacing: 1px;
  font-size: 6em;
}

.links {
  padding-top: 15px;
}
</style>
