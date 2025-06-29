<template>
  <PageStructure title="Events">
    <CollapseSection collapse-title="Filter by Date" open>
      <div
        class="flex flex-col gap-[var(--spacing-in-sections)] items-end w-full"
      >
        <Text v-if="selectedDate" @click="clearSelectedDate"
          >Remove Filter</Text
        >
        <Calendar @date-selected="filterByDate" :selected-date="selectedDate" />
      </div>
    </CollapseSection>

    <Card
      v-for="event in selectedDate ? filtered : result"
      :key="event.id"
      :card-image="event.img ? baseUrl + event.img : EventBanner"
      card-image-alt="Event Banner"
      :card-title="event.name"
      :card-date="event.date"
      :card-text="event.description"
      :button-title="
        isUserSignedUp(event.id?.toString() ?? '')
          ? 'Already Signed Up'
          : 'Sign Up'
      "
      :button-class="
        isUserSignedUp(event.id?.toString() ?? '') ? '!btn-accent' : ''
      "
      :link="`/events/${event.id}`"
      vertical
    />
  </PageStructure>
</template>
<script setup lang="ts">
import Card from "../components/Card.vue";
import { ref, watch } from "vue";
import { useApiData } from "../api/useApiRequest.ts";
import type { Events } from "../types/types.ts";
import EventBanner from "/img/event-banner-ai.png";
import Calendar from "../components/Calendar.vue";
import Text from "../components/Text.vue";
import CollapseSection from "../components/CollapseSection.vue";
import PageStructure from "../components/PageStructure.vue";
import { isUserSignedUp } from "../utils/helpers.ts";

const result = ref<Events[]>([]);
const filtered = ref<Events[]>([]);
const selectedDate = ref<string | null>(null);
const baseUrl = import.meta.env.VITE_API_BASE_URL || "http://localhost:8000";

const { data } = useApiData<Events[]>("events");

function filterByDate(date: string) {
  selectedDate.value = date;
  filtered.value = result.value.filter((event) => event.date.startsWith(date));
}

function clearSelectedDate() {
  selectedDate.value = null;
  filtered.value = [];
}

watch(data, () => {
  if (data.value) {
    result.value = data.value;
  }
});
</script>
