<script setup lang="ts">
import { ref, toRefs } from "vue";
import { RouterLink } from "vue-router";
import uniqolor from "uniqolor";

const props = defineProps<{
  category: string;
  image: string;
}>();

const { category, image } = toRefs(props);
let randColor = ref("");
randColor.value = uniqolor.random();
console.log(randColor.value.color);
</script>

<template>
  <RouterLink to="/library">
    <div
      v-if="randColor.color"
      :style="`background-color: ${randColor.color};`"
      :class="`contrast-[2] brightness-[0.87] rounded-xl aspect-square relative overflow-hidden`"
    >
      <div class="text-white pt-3 pl-4 font-semibold text-[23]">
        {{ category }}
      </div>
      <img
        :src="image || ''"
        class="absolute contrast-[0.55] -right-3.5 bottom-0 max-w-[100px] rotate-[20deg]"
      />
    </div>
  </RouterLink>
</template>
