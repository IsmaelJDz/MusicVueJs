<template lang="pug">
#app
    img(src='https://ismaeljdz.github.io/MusicVueJs/dist/logo.png')
    h1 PlatziMusic
    select(v-model="selectedCountry")
        option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
        artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>

import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
          { name: 'Argentina', value: 'argentina' },
          { name: 'Colombia', value: 'colombia' },
          { name: 'Mexico', value: 'mexico' }
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components: {
    Artist: Artist,
    Spinner: Spinner
  },
  methods: {
    refreshArtists() {
        const self = this
        this.loading = true
        this.artists = []
        getArtists(this.selectedCountry)
          .then(function (artists){
            self.loading = false
            self.artists = artists
        })
    }
  },
  mounted: function () {
      this.refreshArtists()
  },
  watch: {
      selectedCountry: function () {
          this.refreshArtists()
      }
  }
}
</script>

<style lang="stylus" scoped>
    #app
        font-family 'Avenir', Helvetica, Arial, sans-serif
        -webkit-font-smoothing antialiased
        -moz-osx-font-smoothing grayscale
        text-align center
        color red !important
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
