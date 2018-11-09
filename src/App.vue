<template>
  <div id="app">
    <PmHeader></PmHeader>
    <PmNotification v-show="showNotification">
      <p slot="body">No se encontraron resultados</p>
    </PmNotification>
    <section class="section">
      <nav class="navbar">
        <div class="container">
          <div class="field has-addons">
            <div class="control">
              <input class="input is-large"
                type="text"
                placeholder="¿Qué canción estás buscando?"
                v-model="searchQuery">
            </div>
            <div class="control">
              <a class="button is-info is-large"
                v-on:click="search" :class="{ 'is-loading': isLoading }">
                Buscar
              </a>
            </div>
            <div class="control">
              <a class="button is-danger is-large"> 
                &times;
              </a>
            </div>
          </div>
        </div>
      </nav>
      <!--PmLoader v-show="isLoading"></PmLoader-->
      <div class="container" v-show="!isLoading">
        <p class="help is-info has-text-right">
          <small> {{ searchMessage }} </small>
        </p>
      </div>
      <div class="container results" v-show="!isLoading">
        <div class="columns is-multiline">
          <div class="column is-one-quarter" 
            v-for="t in tracks" :key="t.name">
            <PmTrack 
              :class="{ 'is-active': t.id == selectedTrack }"
              :track="t" 
              @select="setSelectedTrack">
            </PmTrack>
          </div>
        </div>
      </div>
    </section>
    <PmFooter></PmFooter>
  </div>
</template>

<script>
import trackService from '@/services/track'
import PmFooter from '@/components/layout/Footer.vue'
import PmHeader from '@/components/layout/Header.vue'

import PmTrack from '@/components/Track.vue'

import PmLoader from '@/components/shared/Loader.vue'
import PmNotification from '@/components/shared/Notification.vue'

export default {
  name: 'app',

  components: { 
    PmFooter, 
    PmHeader, 
    PmTrack, 
    PmLoader,
    PmNotification },

  data () {
    return {
      searchQuery: '',
      tracks: [],
      isLoading: false,
      showNotification: false,
      selectedTrack: ''
    }
  },

  computed: {
    searchMessage () {
      return `Encontrados: ${this.tracks.length}`
    }
  },

  watch: {
    showNotification () {
      if (this.showNotification) {
        setTimeout(() => {
          this.showNotification = false
        }, 3000);
      }
    }
  },

  methods: {
    search () {
      if (!this.searchQuery) { return }

      this.isLoading = true

      trackService.search(this.searchQuery)
        .then(res => {
          this.showNotification = res.tracks.total === 0
          this.tracks = res.tracks.items
          this.isLoading = false
        })
    },
    setSelectedTrack (id) {
      this.selectedTrack = id
    }
  }
}
</script>

<style lang="scss">
  @import './scss/main.scss';

  .results {
    margin-top: 50px;
  }

  .is-active {
    border: 3px #23d160 solid;
  }
</style>