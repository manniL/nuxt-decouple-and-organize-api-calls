<template>
  <section>
    <h1>{{ post.title }}</h1>
    <p>{{ post.body }}</p>
    <p>by User with id {{ post.userId }}</p>
  </section>
</template>

<script>
export default {
  data() {
    return {
      post: {}
    }
  },
  async asyncData({ app, params }) {
    return {
      post: await app.$postRepository.show(params.id)
    }
  }
}
</script>
