<template>
  <div
    class="main-story-container fill-screen flex-vertical"
    v-if="!storySelected"
  >
    <story-line-container
      v-for="(story, index) in stories"
      :title="story.title"
      :avatar="story.avatar"
      :index="index"
      :sections="story.sections"
      type="otherStory"
      :key="index"
    />
  </div>
  <router-view v-else />
</template>

<script setup lang="ts">
import { computed } from "vue";
import { useRoute } from "vue-router";
import StoryLineContainer from "./story/StoryLineContainer.vue";
import { stories } from "@index/otherStoryIndex";

const route = useRoute();

const storySelected = computed(() => !/\/otherStory\/?$/.test(route.path));
</script>

<style scoped lang="scss">
.main-story-container {
  margin-top: 24px;
}
</style>
