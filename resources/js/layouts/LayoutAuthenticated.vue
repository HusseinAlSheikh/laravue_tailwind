<script setup>
import { computed } from 'vue'
import { useLayoutStore } from '@/stores/layout.js'
import { useStyleStore } from '@/stores/style.js'
import menu from '@/menu.js'
import NavBar from '@/components/NavBar.vue'
import AsideMenu from '@/components/AsideMenu.vue'
import FooterBar from '@/components/FooterBar.vue'
import OverlayLayer from '@/components/OverlayLayer.vue'

const styleStore = useStyleStore()

const layoutStore = useLayoutStore()

const isAsideLgActive = computed(() => layoutStore.isAsideLgActive)

const overlayClick = () => {
  layoutStore.isAsideLgActive = false
};

import { router } from "@inertiajs/vue3";

router.on("navigate", () => {
 // isAsideMobileExpanded.value = false;
  isAsideLgActive.value = false;
});

// Replace `isLogout` logic:

const menuClick = (event, item) => {
  // ...

  if (item.isLogout) {
    // Add:
    router.post(route("logout"));
  }
};

</script>

<template>
  <div :class="{ 'dark': styleStore.darkMode, 'overflow-hidden lg:overflow-visible': layoutStore.isAsideMobileExpanded }">
    <div
      :class="{ 'ml-60 lg:ml-0': layoutStore.isAsideMobileExpanded }"
      class="pt-14 xl:pl-60 w-screen transition-position lg:w-auto bg-gray-50 dark:bg-slate-800 dark:text-slate-100"
    >
      <NavBar />
      <AsideMenu :menu="menu" />
      <slot />
      <FooterBar />
      <OverlayLayer
        v-show="isAsideLgActive"
        z-index="z-30"
        @overlay-click="overlayClick"
      />
    </div>
  </div>
</template>
