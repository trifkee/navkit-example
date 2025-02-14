<script setup lang="ts">
  import { computed, inject, watch, type Ref } from "vue";

  import { useNavigation, useScrollIntoFocus } from "navkit-vue";

  import Carousel from "./Carousel.vue";

  import { movies } from "@/assets/movies";

  import type { MovieType } from "@/App.vue";

  // Logic
  const props = defineProps<{
    active: {
      curr: "hero" | "nav" | "carousels";
      last: "hero" | "nav" | "carousels";
    };
  }>();

  const emit = defineEmits(["update-active"]);

  const rows = Object.values(movies).map((movie) => movie.length);

  const isDisabled = computed(() => props.active.curr !== "carousels");

  const { position, getCurrentFocusedElement } = useNavigation({
    rows,
    focusableSelector: "[data-movie-carousel]",
    holdColumnPerRow: true,
    disabled: isDisabled,
    onReturn: () => {
      emit("update-active", "nav");
    },
    onRowStart: () => {
      emit("update-active", "hero");
    },
    onColumnStart: () => {
      emit("update-active", "nav");
    },
  });

  const currElement = computed(() => getCurrentFocusedElement());

  const { handleChangeMovie } = inject("featuredMovie") as {
    movieRef: Ref<MovieType>;
    handleChangeMovie: (movie: MovieType) => void;
  };

  // Change featured movie with debounce
  let timeout: ReturnType<typeof setTimeout>;

  watch(position, () => {
    clearTimeout(timeout);

    timeout = setTimeout(() => {
      handleChangeMovie(
        (movies as any)[position.value.row][position.value.col]
      );
    }, 500);
  });

  useScrollIntoFocus({
    position,
    selectedElement: currElement,
    parentSelector: "[data-parent]",
    buffer: 40,
    scrollType: "throttle",
    suppressLogs: false,
    delay: 50,
  });
</script>

<template>
  <div class="n" data-parent>
    <Carousel
      v-for="(movie, i) in movies"
      :key="i"
      :movies="movie"
      title="Carousel Title"
    />
  </div>
</template>

<style lang="scss" scoped>
  .n {
    padding-bottom: 1rem;
    overflow: auto;
    display: flex;
    flex-direction: column;
    gap: 1.25rem;
  }
</style>
