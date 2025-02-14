<script setup lang="ts">
  import { provide, ref } from "vue";

  import Featured from "./components/Featured.vue";
  import MoreCarousels from "./components/MoreCarousels.vue";
  import Navigation from "./components/Navigation.vue";
  import { movies } from "./assets/movies";

  // Types
  export type MovieType = {
    title: string;
    year: string;
    genre: string;
    duration: string;
    rating: string;
    description: string;
    image: string;
  };

  // Logic
  const active = ref<{
    curr: "hero" | "nav" | "carousels";
    last: "hero" | "nav" | "carousels";
  }>({
    curr: "hero",
    last: "hero",
  });

  function updateActive(val: "hero" | "nav" | "carousels") {
    active.value.last = active.value.curr;
    active.value.curr = val;
  }

  const movie: MovieType = movies[0][0];

  const movieRef = ref(movie);

  const handleChangeMovie = (movieVal: MovieType) => {
    movieRef.value = movieVal;
  };

  provide("featuredMovie", {
    movieRef,
    handleChangeMovie,
  });
</script>

<template>
  <div class="app">
    <Navigation :active="active" @updateActive="updateActive" />
    <div class="right">
      <Featured :active="active" @updateActive="updateActive" />
      <MoreCarousels :active="active" @updateActive="updateActive" />
    </div>
  </div>
</template>

<style lang="scss" scoped>
  .app {
    display: flex;
    height: 100vh;
    width: 100vw;
    overflow: hidden;
    background: #1f1f1f;
  }

  .right {
    flex: 1;
    max-height: 1080px;
    display: flex;
    flex-direction: column;

    & > *:first-child {
      height: 60vh;
    }
    & > *:last-child {
      max-height: 40vh;
    }
  }
</style>
