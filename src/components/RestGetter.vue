<template>
<div class="container-fluid">
  <div class="row">
    <div class="col-md-4">
      <button class="btn btn-success btn-block" @click='fetchDataFrom("https://css-tricks.com")'>csstricks.com</button>
    </div>
    <div class="col-md-4">
      <button class="btn btn-warning btn-block" @click='fetchDataFrom("https://codecondo.com")'>codecondo.com</button>
    </div>
    <div class="col-md-4">
      <button class="btn btn-info btn-block" @click='fetchDataFrom("https://blog.codepen.io")'>blog.codepen.io</button>
    </div>
</div>
<hr>
<div class="row justify-content-center">
<div class="col-md-8">
  <div class="row">
    <div class="col">
<form @submit.prevent="fetchData">
      <div class="input-group mb-2">
        <div class="input-group-prepend">
          <div class="input-group-text">Request from custom url:</div>
        </div>
          <input type="text" class="form-control" id="inlineFormInputGroup" v-model="fetchUrl" >
      </div>
      </form>
    </div>

<div class="col-3">
        <transition name='fade'>
  <button class="btn btn-danger btn-block" v-if='posts.length > 0' @click='clearPosts'>Clear Posts</button>
  </transition>
</div>



  </div>

    </div>


</div>

<transition name='fade'>
  <div class="row mt-3" v-if='loading === true'>
    <div class="col-md-12 text-center">
      <i class='fas fa-circle-notch fa-spin fa-7x'></i>
    </div>
  </div>
</transition>


<div class="row">
<transition name="fadeSlow">
<div class="col-xs-12 col-sm-12 rounded" v-if='posts.length > 0'>
  <h2 class="text-center">Posts From: {{ fetchUrl.replace(/^(?:https?:\/\/)?(?:www\.)?/i, "").split('/')[0] }}</h2>
<ul class="list-group p-3">
  <li class="list-group-item shadow" :key="post" v-for="post in posts.slice().reverse()">
<h3>{{ post.title.rendered }}</h3>
<p class="small">{{ post.date }}</p>
<p v-html='post.content.rendered'>
{{ post.content.rendered }}
</p>

  </li>
</ul>
  </div>
</transition>
</div>


  </div>

</template>

<script>
export default {

  name: "RestGetter",

  data() {
return {
  posts: [],
  loading: false,
  fetchUrl: 'https://www.woo360.fun'
}
  },

  methods: {

    fetchData(){
      this.loading = true;
      this.posts = [];
      let url = this.fetchUrl + "/wp-json/wp/v2/posts";

      this.$http.get(url)
      .then(response => {
        return response.json();
      })
      .then(data => {
        const resultArray = [];
        for (let key in data) {
          resultArray.push(data[key])
        }
        this.posts = resultArray;
        this.loading = false;
      });

    },

    fetchDataFrom(customUrl){
      this.loading = true;
      this.posts = [];
      this.fetchUrl = customUrl;
      let url = customUrl + "/wp-json/wp/v2/posts";

      this.$http.get(url)
      .then(response => {
        return response.json();
      })
      .then(data => {
        const resultArray = [];
        for (let key in data) {
          resultArray.push(data[key])
        }
        this.posts = resultArray;
        this.loading = false;
      });

    },

    clearPosts(){
      setTimeout(()=>{
      this.posts = [];
      this.loading = false;
      },250);

    }

  },

  created: function() {

      this.loading = true;
      let url = this.fetchUrl + "/wp-json/wp/v2/posts";

      this.$http.get(url)
      .then(response => {
        return response.json();
      })
      .then(data => {
        const resultArray = [];
        for (let key in data) {
          resultArray.push(data[key])
        }
        this.posts = resultArray;
        this.loading = false;
      });

    }

};
</script>

<style>
.container-fluid {
  height: 1200px;
}

.fade-enter {
  opacity: 0;
}
.fade-enter-active {
  transition: 1s;
}

.fade-leave-active {
  transition: 1s;
  opacity: 0;
}

.fadeFast-enter {
  opacity: 0;
}
.fadeFast-enter-active {
  transition: 1s;
}

.fadeFast-leave-active {
  transition: 1s;
  opacity: 0;
}

.fadeSlow-enter {
  opacity: 0;
}
.fadeSlow-enter-active {
  transition: 1s;
  transition-delay: 1s;
}

.fadeSlow-leave-active {
  transition: 1s;
  opacity: 0;
}

img {
  width: 100%;
  height: auto;
}
</style>
