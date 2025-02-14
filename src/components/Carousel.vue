<script setup lang="ts">
  defineProps<{
    movies: {
      id: number;
      title: string;
      year: string;
      rating: string;
      image: string;
    }[];
    title: string;
  }>();
</script>

<template>
  <section class="trending">
    <h2 class="trending__title">{{ title }}</h2>
    <div class="carousel">
      <div class="carousel__content">
        <div
          data-movie-carousel
          v-for="movie in movies"
          :key="movie.id"
          class="movie-card"
        >
          <div
            :style="`--img: url(${movie.image})`"
            class="movie-card__image"
            :alt="movie.title"
          />
          <div class="movie-card__info">
            <h3 class="movie-card__title">{{ movie.title }}</h3>
            <div class="movie-card__meta">
              <span class="movie-card__year">{{ movie.year }}</span>
              <span class="movie-card__rating">{{ movie.rating }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
  .trending {
    // padding: 2rem;

    &__title {
      padding-inline: 1rem;
      font-size: 1.5rem;
      font-weight: bold;
      color: #fff;
      margin-bottom: 0.5rem;
    }
  }

  .carousel {
    position: relative;
    padding-inline: 1rem;
    // width: 100%;
    overflow-x: auto;

    /* Hide scrollbar but keep functionality */
    scrollbar-width: none; /* Firefox */
    -ms-overflow-style: none; /* IE and Edge */
    &::-webkit-scrollbar {
      display: none; /* Chrome, Safari, Opera */
    }

    &__content {
      display: flex;
      gap: 1rem;
      padding: 0.5rem;
      /* Add extra space at the end for last card */
      padding-right: 2rem;
    }
  }

  .movie-card {
    flex: 0 0 auto;
    // width: 300px;
    // height: 250px;
    width: 250px;
    background: #1a1a1a;
    border-radius: 0.5rem;
    // overflow: hidden;
    transition: transform 0.3s ease;
    cursor: pointer;
    aspect-ratio: 3/4;
    display: flex;
    flex-direction: column;
    outline: 2px solid transparent;
    transition: 0.2s ease all;
    isolation: isolate;
    position: relative;
    overflow: hidden;

    &__image {
      width: 100%;
      object-fit: cover;
      background-image: linear-gradient(0deg, rgb(0, 0, 0, 1), transparent),
        var(--img);
      background-size: cover;
      background-position: center;
      position: absolute;
      z-index: -1;
      height: 100% !important;

      object-position: center;

      &::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: linear-gradient(
          180deg,
          rgba(0, 0, 0, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        );
        z-index: 20;
        transition: background 0.3s ease;
      }
    }

    &.focused {
      outline: 2px solid #db2777;
      transform: translateY(-5px);
    }

    &__image {
      width: 100%;
      height: 300px;
      object-fit: cover;
    }

    &__info {
      margin-top: auto;
      padding: 1rem;
    }

    &__title {
      margin-top: auto;
      color: #fff;
      font-size: 1.25rem;
      font-weight: 500;
      margin-bottom: 0.5rem;
      text-overflow: ellipsis;
    }

    &__meta {
      display: flex;
      justify-content: space-between;
      color: #999;
      font-size: 0.875rem;
    }

    &__year,
    &__rating {
      color: #999;
    }
  }
</style>
