<template>
  <Layout :is-home="true">
    <!-- List posts -->
    <div class="posts">
      <PostCard v-for="edge in postWoMeetup" :key="edge.node.id" :post="edge.node" />
    </div>
  </Layout>
</template>

<page-query>
query {
  posts: allPost(filter: { published: { eq: true }}) {
    edges {
      node {
        id
        title
        date (format: "D MMM YYYY")
        timeToRead
        description
        cover_image (width: 770, height: 380, blur: 10)
        path
        author {
          id
          name
          link
          title
          avatar
          path
        }
        tags {
          id
          title
          path
        }
      }
    }
  }
}
</page-query>

<script>
import PostCard from '~/components/PostCard.vue'

export default {
  components: {
    PostCard,
  },
  metaInfo: {
    title: 'Beranda Blog | Vue.js Indonesia',
  },
  computed: {
    postWoMeetup: function () {
      return this.$page.posts.edges.filter((post) => {
        const postTags = post.node.tags
        const isIncludeMeetup = postTags.find((tag) => tag.id === 'Meetup')
        return !isIncludeMeetup
      })
    },
  },
}
</script>
