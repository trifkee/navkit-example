<script setup lang="ts">
  import { computed, defineProps, defineEmits, inject, type Ref } from "vue";
  import { useNavigationX } from "navkit-vue";

  import type { MovieType } from "@/App.vue";

  const props = defineProps<{
    active: {
      curr: "hero" | "nav" | "carousels";
      last: "hero" | "nav" | "carousels";
    };
  }>();

  const { movieRef } = inject("featuredMovie") as {
    movieRef: Ref<MovieType>;
    handleChangeMovie: (movie: MovieType) => void;
  };

  const emit = defineEmits(["update-active"]);

  // Computed property to track the disabled state
  const isDisabled = computed(() => props.active.curr !== "hero");

  function handleChange(type: string) {
    emit("update-active", type);
  }

  useNavigationX({
    columns: 3,
    disabled: isDisabled,
    focusableSelector: "[data-featured]",
    onColumnStart: () => {
      handleChange("nav");
    },
    onDown: () => {
      handleChange("carousels");
    },
    onReturn: () => {
      handleChange("nav");
    },
  });
</script>

<template>
  <div class="movie-detail">
    <!-- Hero Section with Background -->
    <div class="hero">
      <!-- Background Image -->
      <div class="hero__background">
        <img :src="movieRef.image" alt="" />
        <!-- Gradient Overlay -->
        <div class="hero__overlay"></div>
      </div>

      <!-- Content -->
      <div class="hero__content">
        <!-- Movie Info -->
        <div class="movie-info">
          <h1 class="movie-info__title">{{ movieRef.title }}</h1>

          <!-- Meta Info -->
          <div class="movie-info__meta">
            <span>{{ movieRef.year }}</span>
            <span>{{ movieRef.genre }}</span>
            <span>{{ movieRef.duration }}</span>
            <div class="rating">
              <span>{{ movieRef.rating }}</span>
              <span>Ratings</span>
            </div>
          </div>

          <!-- Description -->
          <p class="movie-info__description">
            {{
              movieRef.description ??
              "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
            }}
          </p>

          <!-- Action Buttons -->
          <div class="quick-actions">
            <button class="action-button">
              <span>Trailer</span>
            </button>
            <button class="action-button">
              <span>Watchlist</span>
            </button>
            <button class="action-button">
              <span>Watch Party</span>
            </button>
            <button class="action-button">
              <span>Share</span>
            </button>
          </div>

          <!-- Main Actions -->
          <div class="main-actions">
            <button data-featured class="btn btn--primary">
              Watch with us
            </button>
            <button data-featured class="btn btn--secondary">
              Rent Movie HD $4.99
            </button>
            <button data-featured class="btn btn--secondary">
              More Purchase Options
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
  .movie-detail {
    width: 100%;
    height: 60vh;
    background-color: #000;
    color: #fff;
    position: relative;

    &::before {
      content: "";
      height: 100%;
      width: 100%;
      background: radial-gradient(
          circle at top right,
          rgba(255, 255, 255, 0) 0%,
          #1f1f1f 90%
        ),
        linear-gradient(0deg, #1f1f1f, transparent);

      z-index: 1;
      position: absolute;
      left: 0;
      top: 0;
    }
  }

  .hero {
    position: relative;
    height: 60vh;

    &__background {
      position: absolute;
      inset: 0;

      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
    }

    &__overlay {
      position: absolute;
      inset: 0;
      background: linear-gradient(to right, rgba(0, 0, 0, 0.8), transparent);
    }

    &__content {
      position: relative;
      z-index: 1;
      height: 100%;
      display: flex;
      flex-direction: column;
      justify-content: flex-end;
      padding: 2rem;
    }
  }

  .movie-info {
    max-width: 48rem;

    &__title {
      font-size: 2.5rem;
      font-weight: bold;
      margin-bottom: 0.5rem;
    }

    &__meta {
      display: flex;
      align-items: center;
      gap: 1rem;
      color: #ccc;
      font-size: 0.875rem;
      margin-bottom: 1rem;
    }

    &__description {
      color: #ccc;
      margin-bottom: 1.5rem;
      max-width: 32rem;
      min-height: 70px;
    }
  }

  .quick-actions {
    display: flex;
    align-items: center;
    gap: 1.5rem;
    margin-bottom: 2rem;
  }

  .action-button {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
    background: none;
    border: none;
    color: #ccc;
    cursor: pointer;
    transition: color 0.3s;

    &:hover {
      color: #fff;
    }
  }

  .main-actions {
    display: flex;
    align-items: center;
    gap: 1rem;
  }

  .btn {
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 0.5rem;
    cursor: pointer;
    font-size: 1rem;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    transition: opacity 0.3s;

    &.focused {
      outline: none;
      background: #db2777;
    }

    &:hover {
      opacity: 0.9;
    }

    &--primary {
      background-color: rgba(255, 255, 255, 0.2);
      color: #fff;
    }

    &--secondary {
      background-color: rgba(255, 255, 255, 0.2);
      color: #fff;
      backdrop-filter: blur(4px);
    }
  }

  .trending {
    padding: 2rem;

    &__title {
      font-size: 1.5rem;
      font-weight: bold;
      color: #fff;
      margin-bottom: 1rem;
    }
  }

  .rating {
    display: flex;
    align-items: center;
    gap: 0.25rem;
  }
</style>
