<template>
  <PageStructure title="Research">
    <div
      class="w-full flex justify-between items-center gap-[var(--spacing-in-sections)]"
    >
      <label class="input w-full">
        <svg
          class="h-[1em] opacity-50"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
        >
          <g
            stroke-linejoin="round"
            stroke-linecap="round"
            stroke-width="2.5"
            fill="none"
            stroke="currentColor"
          >
            <circle cx="11" cy="11" r="8"></circle>
            <path d="m21 21-4.3-4.3"></path>
          </g>
        </svg>
        <input
          v-model="searchQuery"
          type="search"
          class="grow"
          placeholder="Search"
        />
      </label>
      <FilterModal
        v-if="data?.find((d) => d.keywords)"
        :selected-keywords="selectedKeywords"
        ref="keywordModal"
        :keywords="data?.map((d) => d.keywords ?? '') ?? []"
        @filter="applyKeywordFilter"
      />
    </div>
    <Card
      v-for="research in filteredResults"
      :key="research.id"
      :card-image="ResearchBanner"
      card-image-alt="Research Banner"
      :card-title="research.name"
      :card-text="research.summary"
      button-title="Learn more"
      :link="'/research/' + research.id?.toString()"
    />
  </PageStructure>
</template>
<script setup lang="ts">
import Card from "../components/Card.vue";
import { computed, onMounted, ref, watch } from "vue";
import { useApiData } from "../api/useApiRequest.ts";
import type { Research } from "../types/types.ts";
import ResearchBanner from "/img/research-paper-banner-ai.png";
import PageStructure from "../components/PageStructure.vue";
import FilterModal from "../components/FilterModal.vue";
import useSearchAndFilter from "../../composables/useSearchAndFilter.ts";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();
const keywordModal = ref();

const { data } = useApiData<Research[]>("research");

const { searchQuery, filteredResults, applyKeywordFilter, selectedKeywords } =
  useSearchAndFilter(
    computed(() => data.value ?? []),
    (research: Research) => [research.name, research.summary ?? ""],
  );

/**
 * On load the search query and selected keywords are set from the URL.
 * This allows redirecting to the page from another page and executing a search or sharing a link with the search and filter applied.
 */
onMounted(() => {
  searchQuery.value = route.query.search?.toString() || "";
  selectedKeywords.value = route.query.keywords
    ? route.query.keywords.toString().split(",")
    : [];
});

watch(searchQuery, (newQuery) => {
  router.replace({
    query: {
      ...route.query,
      search: newQuery || undefined,
    },
  });
});

watch(selectedKeywords, (newKeywords) => {
  router.replace({
    query: {
      ...route.query,
      keywords: newKeywords.length ? newKeywords.join(",") : undefined,
    },
  });
});
</script>
