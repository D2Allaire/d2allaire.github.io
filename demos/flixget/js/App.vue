<template>
  <div id="app">
    <div class="title">
      <h2 v-cloak>{{ movie.title }} </h2>
      <ul v-cloak>
        <li>{{ movie.year }}</li>
        <li>{{ movie.runtime }}</li>
        <li>{{ language }}</li>
        <li>{{ movie.genre }}</li>
      </ul>
    </div>
    <div class="poster" ref="poster">
    </div>
    <div class="meta">
      <div class="container">
        <div class="meta__description">
          <h3 v-cloak>Description</h3>
          <p>{{ movie.plot }}</p>
        </div>
        <div class="clear"></div>
        <div class="meta__rating">
          <h3>Rating</h3>
          <p>IMDB: <span class="rating--score" v-cloak>{{ movie.imdb_rating }}</span></p>
          <p><a href="#">See entry on IMDB</a></p>
        </div>
        <div class="clear"></div>
        <div class="meta__countries">
          <h3>Available Countries</h3>
          <div class="meta_flags">
          </div>
        </div>
      </div>
    </div>
    <div class="clear"></div>
    <div class="action container">
      <select v-model="selected">
        <option v-for="region in regions" :value="region.code" :selected="region.code === 'us' ? 'selected' : ''">{{ region.name }}</option>
      </select>
      <div class="clear"></div>
      <div class="action__buttons">
        <button class="button button--secondary">Watch on Netflix</button>
        <button class="button button--primary" @click="loadMovie">Suggest New</button>
      </div>
    </div>
  </div>
</template>
<script>
var $ = window.jQuery = require('jquery')
require('../static/vendor/jquery.nice-select.min.js')

export default {
  name: 'app',
  data () {
    return {
      movie: {
        title: "Charlie's Country",
        year: '2013',
        runtime: '104 min',
        language: [],
        genre: 'Drama',
        imdb_rating: '7.3',
        plot: 'No plot LUL very long string though I will just type my heart out on this one. I always wanted to write a novel, noew this is my chance. Finally!'
      },
      regions: [],
      selected: ''
    }
  },

  computed: {
    language () {
      if (this.movie.language.indexOf('English') > -1) return 'English'
      else return this.movie.language[0]
    }
  },

  mounted: function () {
    this.loadMovie()
    this.loadRegions()
    $('select').niceSelect()
  },

  methods: {
    loadMovie () {
      const region = $('.selected:first').data('value') || 'us'
      this.$http.get('/api/movies?r=' + region)
        .then((response) => {
          console.log(JSON.stringify(response.body))
          this.movie = response.body[0]
          this.$refs.poster.style.backgroundImage = 'url(' + response.body[0].poster + ')'
        })
    },
    loadRegions () {
      this.$http.get('/api/regions')
        .then((response) => {
          console.log(response.body)
          this.regions = response.body
          this.$nextTick(function () {
            $('select').niceSelect('update')
          })
        })
    }
  }
}

</script>
