<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h1 Platzimusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" :key="artist.mbid")

</template>

<script>
import Artist from './components/Artist'
import Spinner from './components/Spinner'
import getArtist from './api'

export default {
  name: 'app',
  data () {
    return {
      artists: [ ],
      countries: [
        {name: "Guatemala", value: "Guatemala"},
        {name: "Argentina ", value: "Argentina"},
        {name: "España", value: "Spain"},
      ],
      selectedCountry: 'Guatemala',
      loading: true
    }
  },
  components: {
    Artist,//: Artist
    Spinner
  },
  methods: {
    refreshArtist: function () {
      const self = this
      this.loading = true
      this.artists = []
      getArtist(this.selectedCountry)
        .then(function (art) {
          self.loading = false
          self.artists = art
        })
     }   
  },
  mounted: function () {
    this.refreshArtist()
  },
  watch: {
    selectedCountry: function () {
      this.refreshArtist()
    }
  }
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
