<template>
  <div class="flex flex-col gap-[var(--spacing-in-sections)]">
    <Picture
      :image="NotFoundPicture"
      image-alt="Not Found Picture"
      picture-class="rounded"
    />
    <Heading heading="h1">Not Found</Heading>
    <Text>Sorry, the page you are looking for does not exist.</Text>
    <router-link to="/">
      <button class="btn btn-primary w-full">Go back to Home</button>
    </router-link>
  </div>
</template>
<script setup lang="ts">
import Heading from "../components/Heading.vue";
import Text from "../components/Text.vue";
import Picture from "../components/Picture.vue";
import NotFoundPicture from "/img/not-found-portrait-ai.png";
</script>
