<template>
  <section class="new_song">
    <Slider :data="sliderData" class="new_song__slider"/>
    <AppMusicList :music-list="newSongs" class="new_song__music_list"/>
  </section>
</template>

<script>
import AppMusicList from '../public/AppMusicList'
import Slider from './Slider.vue'
import axios from 'axios'
import api from '../../assets/js/api'
import { mapState, mapMutations } from 'vuex'
import loading from '../../mixins/loading'
export default {
  name: 'NewSong',
  mixins: [loading],
  computed: {
    ...mapState('newSong', ['newSongs', 'sliderData'])
  },
  created() {
    if (this.newSongs.length === 0) {
      this.setLoadingExcludeNav()
      this.startLoading()
      this.getNewSong()
    }
  },
  methods: {
    ...mapMutations(['replaceProperty']),
    getNewSong() {
      axios.get(api.newSong).then(({ data }) => {
        this.replaceProperty({
          paths: 'newSong.newSongs',
          data: data.data
        })
        let banners = data.banner
        this.replaceProperty({
          paths: 'newSong.sliderData',
          data: banners
        })
        this.stopLoading()
      })
    }
  },
  components: {
    AppMusicList,
    Slider
  }
}
</script>

<style lang="less" scoped>
.new_song__slider {
  box-sizing: border-box;
  width: 100%;
  height: 154px;
}
</style>
