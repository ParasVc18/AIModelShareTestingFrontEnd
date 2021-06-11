<template>
  <Layout>
    <h2 class="mb-8 text-4xl font-bold text-center capitalize">
      News Section : <span class="text-blue-700">{{ section }}</span>
    </h2>
    <NewsFilter v-model="section" :fetch="fetchNews" />
    <NewsList :posts="posts" />
  </Layout>
</template>

<script>
import Layout from "./components/Layout.vue"
import NewsFilter from "./components/NewsFilter.vue"
import NewsList from "./components/NewsList.vue"

import axios from "axios"
const api = "b74e242827df443a9d897808ac5f1806"

export default {
  components: {
    Layout,
    NewsFilter,
    NewsList,
  },
  data() {
    return {
      section: "home",
      posts: [],
    }
  },
  methods: {
    async fetchNews() {
      try {
        const url = `https://newsapi.org/v2/everything?q=${this.section}&apiKey=${api}`
        const response = await axios.get(url)
        const results = response.data.articles
        this.posts = results.map(post => ({
          title: post.title,
          abstract: post.description,
          url: post.url,
          thumbnail: post.urlToImage,
          published_date: post.publishedAt,
        }))
      } catch (err) {
        if (err.response) {
          // client received an error response (5xx, 4xx)
          console.log("Server Error:", err)
        } else if (err.request) {
          // client never received a response, or request never left
          console.log("Network Error:", err)
        } else {
          console.log("Client Error:", err)
        }
      }
    },
  },
  mounted() {
    this.fetchNews()
  },
}
</script>
