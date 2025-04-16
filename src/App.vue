<template>
  <div id="app">
    <HeaderBar />
    <!-- <div class="main-content"> -->
    <!--   <Sidebar> -->
    <!--     <ControlPanel -->
    <!--       :macroLayerVisible="macroLayerVisible" -->
    <!--       :macroDropdownVisible="macroDropdownVisible" -->
    <!--       :neighborVisible="neighborVisible" -->
    <!--       :notNeighborVisible="notNeighborVisible" -->
    <!--       :microLayerVisible="microLayerVisible" -->
    <!--       :microDropdownVisible="microDropdownVisible" -->
    <!--       :intervalVisible="intervalVisible" -->
    <!--       :monophonicVisible="monophonicVisible" -->
    <!--       @toggleMacroLayer="toggleMacroLayer" -->
    <!--       @toggleMacroDropdown="toggleMacroDropdown" -->
    <!--       @toggleMicroLayer="toggleMicroLayer" -->
    <!--       @toggleMicroDropdown="toggleMicroDropdown" -->
    <!--       @toggleNeighbor="toggleNeighbor" -->
    <!--       @toggleNotNeighbor="toggleNotNeighbor" -->
    <!--       @toggleInterval="toggleInterval" -->
    <!--       @toggleMonophonic="toggleMonophonic" -->
    <!--     /> -->
    <!--     <DescriptionPanel :type="selectedType" :id="selectedId" /> -->
    <!--   </Sidebar> -->
    <!---->
    <!--   <InteractiveCanvas -->
    <!--     :scale="scale" -->
    <!--     :currentTime="currentTime" -->
    <!--     :highlightedImages="highlightedImages" -->
    <!--     :neighborVisible="neighborVisible" -->
    <!--     :notNeighborVisible="notNeighborVisible" -->
    <!--     :intervalVisible="intervalVisible" -->
    <!--     :monophonicVisible="monophonicVisible" -->
    <!--     @showExplanation="showExplanation" -->
    <!--     @zoomIn="zoomIn" -->
    <!--     @zoomOut="zoomOut" -->
    <!--   /> -->
    <!-- </div> -->
    <!---->
    <!-- <MusicPlayer -->
    <!--   :audio="audio" -->
    <!--   :isPlaying="isPlaying" -->
    <!--   :currentTime="currentTime" -->
    <!--   :audioDuration="audioDuration" -->
    <!--   @togglePlay="togglePlay" -->
    <!--   @seek="seek" -->
    <!-- /> -->
  </div>
</template>

<script>
import HeaderBar from './components/HeaderBar.vue'
import Sidebar from './components/Sidebar.vue'
import ControlPanel from './components/ControlPanel.vue'
import DescriptionPanel from './components/DescriptionPanel.vue'
import InteractiveCanvas from './components/InteractiveCanvas.vue'
import MusicPlayer from './components/MusicPlayer.vue'

export default {
  name: 'App',
  components: {
    HeaderBar,
    Sidebar,
    ControlPanel,
    DescriptionPanel,
    InteractiveCanvas,
    MusicPlayer
  },
  data() {
    return {
      scale: 1,
      macroLayerVisible: true,
      macroDropdownVisible: true,
      microLayerVisible: true,
      microDropdownVisible: true,
      neighborVisible: true,
      notNeighborVisible: true,
      intervalVisible: true,
      monophonicVisible: true,
      selectedType: 0,
      selectedId: 1,
      audio: new Audio('/materials/music/horse.mp3'),
      isPlaying: false,
      currentTime: 0,
      audioDuration: 0,
      highlightedImages: []
    }
  },
  methods: {
    toggleMacroLayer() {
      this.macroLayerVisible = !this.macroLayerVisible;
      this.neighborVisible = this.macroLayerVisible;
      this.notNeighborVisible = this.macroLayerVisible;
    },
    toggleMacroDropdown() {
      this.macroDropdownVisible = !this.macroDropdownVisible;
    },
    toggleMicroLayer() {
      this.microLayerVisible = !this.microLayerVisible;
      this.intervalVisible = this.microLayerVisible;
      this.monophonicVisible = this.microLayerVisible;
    },
    toggleMicroDropdown() {
      this.microDropdownVisible = !this.microDropdownVisible;
    },
    toggleNeighbor() {
      this.neighborVisible = !this.neighborVisible;
    },
    toggleNotNeighbor() {
      this.notNeighborVisible = !this.notNeighborVisible;
    },
    toggleInterval() {
      this.intervalVisible = !this.intervalVisible;
    },
    toggleMonophonic() {
      this.monophonicVisible = !this.monophonicVisible;
    },
    zoomIn() {
      this.scale = Math.min(this.scale + 0.1, 2);
    },
    zoomOut() {
      this.scale = Math.max(this.scale - 0.1, 1);
    },
    showExplanation(type, id) {
      this.selectedType = type;
      this.selectedId = id;
    },
    togglePlay() {
      if (this.isPlaying) {
        this.audio.pause();
      } else {
        this.audio.play();
      }
      this.isPlaying = !this.isPlaying;
    },
    seek(time) {
      this.audio.currentTime = time;
      this.currentTime = time;
    }
  },
  mounted() {
    this.audio.addEventListener('timeupdate', () => {
      this.currentTime = this.audio.currentTime;
    });
    this.audio.addEventListener('loadedmetadata', () => {
      this.audioDuration = this.audio.duration;
    });
  },
  beforeUnmount() {
    this.audio.pause();
    this.audio.removeEventListener('timeupdate', () => {});
    this.audio.removeEventListener('loadedmetadata', () => {});
  }
}
</script>
