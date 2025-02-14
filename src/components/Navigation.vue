<script setup lang="ts">
  import { computed } from "vue";

  import { useNavigationY } from "navkit-vue";

  import {
    Home,
    Film,
    Tv,
    Search,
    BookMarked,
    Settings,
  } from "lucide-vue-next";

  import "../assets/navigation.scss";

  const props = defineProps<{
    active: {
      curr: "hero" | "nav" | "carousels";
      last: "hero" | "nav" | "carousels";
    };
  }>();

  // Logic
  const menuItems = [
    { icon: Home, label: "Home" },
    { icon: Film, label: "Movies" },
    { icon: Tv, label: "TV Shows" },
    { icon: Search, label: "Search" },
    { icon: BookMarked, label: "Watchlist" },
  ];

  const emit = defineEmits(["update-active"]);

  function handleChange() {
    emit("update-active", props.active.last);
  }
  const isDisabled = computed(() => props.active.curr !== "nav");

  const { getCurrentFocusedElement } = useNavigationY({
    rows: menuItems.length + 1,
    focusableSelector: "[data-nav]",
    disabled: isDisabled,
    onRight: () => {
      handleChange();
    },
    onReturn: () => {
      handleChange();
    },
  });

  // Computed for reactivity
  const currEl = computed(() => getCurrentFocusedElement());
</script>

<template>
  <nav :class="`navigation ${!isDisabled && 'active'}`">
    <div
      class="indicator"
      :style="`
      width:4px;
      height:${currEl?.clientHeight}px;
      position:absolute;
      left:0;
      top:${currEl?.getBoundingClientRect().top}px;
        `"
    ></div>
    <ul class="navigation__main">
      <li class="item" data-nav v-for="item in menuItems" :key="item.label">
        <component :is="item.icon" class="item__icon" />
        <span class="item__label">{{ item.label }}</span>
      </li>
    </ul>

    <div data-nav class="item">
      <Settings class="item__icon" />
      <span class="item__label">Settings</span>
    </div>
  </nav>
</template>

<style lang="sass" scoped></style>
