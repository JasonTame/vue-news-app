<template>
<section class="section">
  <div class="columns">
    <div class="column">
      <h2 class=" is-size-2 has-text-centered">{{titleText}}</h2>
    </div>
  </div>
  <div v-if="loading" class="columns">
    <div class="column">
      <div class="loader"><img src="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/0.16.1/images/loader-large.gif" alt="loader"></div>
    </div>
  </div>
  <div class="columns" v-if="!loading" v-for="posts in processedPosts">
    <div class="column" v-for="post in posts">
      <div class="card">
        <div class="card-image">
          <figure class="image is-4by3">
            <img :src="post.urlToImage">
          </figure>
        </div>
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <p class="title is-4">{{post.title}}</p>
            </div>
          </div>

          <div class="content">
            <p>{{ post.description }}</p>
            <a target="_blank" :href="post.url">Read More</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
</template>

<style>
.loader {
  -webkit-animation: spinAround 500ms infinite linear;
  animation: spinAround 500ms infinite linear;
  border: 4px solid #f6a000;
  border-radius: 290486px;
  border-right-color: transparent;
  border-top-color: transparent;
  content: "";
  display: block;
  height: 1em;
  position: relative;
  width: 1em;
  margin: 0 auto;
  padding: 80px;
}
</style>


<script>
import axios from 'axios'
import {
  EventBus
} from './event-bus.js';

export default {
  name: 'app',

  data() {
    return {
      apiKey: "APIKEY",
      titleText: "Top Headlines",
      results: [],
      loading: true
    }
  },
  mounted() {
    //Top Headlines API request
    this.getPosts();
    EventBus.$on('top-headlines', () => {
      this.getPosts();
      this.titleText = "Top Headlines"
    })
    //Category API request
    EventBus.$on('chose-category', categoryChosen => {
      this.titleText = "Loading..."
      this.results = [];
      this.loading = true;
      let baseUrl = "https://newsapi.org/v2/top-headlines?country=us&";
      let category = "category=" + categoryChosen + "&";
      console.log("Category variable is " + category);
      let url = baseUrl + category + this.apiKey;
      axios.get(url).then((response) => {
        this.results = response.data.articles;
        this.titleText = categoryChosen
      }).catch(error => {
        console.log(error);
      });
      this.loading = false;
      this.titleText = categoryChosen
    })
    //Keyword API Request
    EventBus.$on('chose-keyword', keyword => {
      this.titleText = "Loading..."
      this.results = [];
      this.loading = true;
      let baseUrl = "https://newsapi.org/v2/top-headlines?country=us&";
      let query = "q=" + keyword + "&";
      console.log("Keyword variable is " + query);
      let url = baseUrl + query + this.apiKey;
      axios.get(url).then((response) => {
        this.results = response.data.articles;
        this.titleText = "Results found for" + "'" + keyword + "'";
        if (response.data.articles.length < 1) {
          this.titleText = "No results found for" + "'" + keyword + "'";
        }
      }).catch(error => {
        console.log(error);
      });
      this.loading = false;

    })
  },
  methods: {
    getPosts() {
      let url = 'https://newsapi.org/v2/top-headlines?sources=techcrunch&' + this.apiKey;
      axios.get(url).then((response) => {
        this.results = response.data.articles;
      }).catch(error => {
        console.log(error);
      });
      this.loading = false;
    }
  },
  computed: {
    processedPosts() {
      let posts = this.results;

      // Put Array into Chunks
      let i, j, chunkedArray = [],
        chunk = 4;
      for (i = 0, j = 0; i < posts.length; i += chunk, j++) {
        chunkedArray[j] = posts.slice(i, i + chunk);
      }
      return chunkedArray;
    }
  }
}
</script>
