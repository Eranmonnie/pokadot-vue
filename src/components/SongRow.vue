<script setup lang="ts">
import { ref, toRefs, onMounted } from "vue";
import Play from "vue-material-design-icons/Play.vue";
import Pause from "vue-material-design-icons/Pause.vue";
import Heart from "vue-material-design-icons/Heart.vue";

import { usesongStore } from "@/stores/song";
import { storeToRefs } from "pinia";

const song = usesongStore();
const { isPlaying, currentTrack } = storeToRefs(song);

let isHover = ref(false);
let isTrackTime = ref();

const props = defineProps<{
  artist: object;
  track: object;
  index: number;
}>();

const { artist, track, index } = toRefs(props);

onMounted(() => {
  const audio = new Audio(track.value.path);

  audio.addEventListener("loadedmetadata", () => {
    const duration = audio.duration;
    const mins = Math.floor(duration / 60);
    const seconds = Math.floor(duration % 60);
    isTrackTime.value = mins + ":" + seconds.toString().padStart(2, "0");
  });
});
</script>

<template>
  <li
    class="flex items-center justify-between rounded-md hover:bg-[#2A2929]"
    @mouseenter="isHover = true"
    @mouseleave="isHover = false"
  >
    <div class="flex items-center w-full py-1.5">
      <div v-if="isHover" class="w-[40px] ml-[14px] mr-[6px] cursor-pointer">
        <Play
          v-if="!isPlaying"
          fillColor="#FFFFFF"
          :size="25"
          @click="song.playOrPauseThisSong(artist, track)"
        />
        
        <Play
          v-else-if="isPlaying && currentTrack.name != track.name"
          fillColor="#FFFFFF"
          :size="25"
          @click="song.loadSong(artist, track)"
        />

        <Pause
          v-else
          fillColor="#FFFFFF"
          :size="25"
          @click="song.playOrPauseSong()"
        />
      </div>

      <div v-else class="text-white font-semibold w-[40px] ml-5">
        <span>
          {{ index }}
        </span>
      </div>

      <div>
        <div class="text-white font-semibold">
          {{ track.name }}
        </div>
        <div class="text-sm font-semibold text-gray-400">
          {{ artist.name }}
        </div>
      </div>
    </div>

    <div class="flex items-center">
      <button type="button" v-if="isHover">
        <Heart fillColor="#1BD760" :size="22" />
      </button>

      <div class="text-xs mx-5 text-gray-400" v-if="isTrackTime">
        {{ isTrackTime }}
      </div>
    </div>
  </li>
</template>
