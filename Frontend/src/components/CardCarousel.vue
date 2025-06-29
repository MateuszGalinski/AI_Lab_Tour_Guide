<template>
  <div class="carousel w-full relative">
    <div
      v-for="(item, index) in items"
      :key="item.id"
      class="carousel-item w-full"
      :class="{
        hidden: activeSlide !== index,
        block: activeSlide === index,
      }"
    >
      <Card
        :card-image="bannerImage"
        :card-image-alt="bannerImageAlt || 'Card Banner'"
        :card-title="item.name"
        :card-date="item.date"
        :card-text="
          item.description ||
          placeholderText ||
          'This is a placeholder text. It should be replaced with actual content.'
        "
        :button-title="
          event
            ? isUserSignedUp(item.id?.toString() ?? '')
              ? 'Already Signed Up'
              : 'Sign Up'
            : buttonTitle || 'Learn More'
        "
        :button-class="
          event
            ? isUserSignedUp(item.id?.toString() ?? '')
              ? '!btn-accent'
              : ''
            : ''
        "
        :link="link + item.id"
        :vertical="vertical"
      />
    </div>
  </div>

  <div class="flex w-full justify-center gap-2 py-2">
    <button
      v-for="(item, index) in items"
      :key="'nav-' + item.id"
      class="btn btn-md btn-circle btn-secondary"
      :class="{ 'btn-active': activeSlide === index }"
      @click="activeSlide = index"
    >
      {{ index + 1 }}
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";
import Card from "./Card.vue";
import { isUserSignedUp } from "../utils/helpers.ts";
import type { CardType } from "../types/types.ts";

const props = defineProps<{
  items: CardType[];
  bannerImage: string;
  bannerImageAlt: string;
  placeholderText?: string;
  buttonTitle?: string;
  link: string;
  vertical?: boolean;
  event?: boolean;
}>();

const activeSlide = ref(0);

watch(
  () => props.items,
  (newItems) => {
    if (activeSlide.value >= newItems.length) {
      activeSlide.value = 0;
    }
  },
  { immediate: true },
);
</script>

<style scoped>
.carousel-item {
  transition: opacity 0.3s ease-in-out;
}
</style>
