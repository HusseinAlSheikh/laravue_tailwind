<script setup>
import { useStyleStore } from '@/stores/style.js'
// import { RouterLink } from 'vue-router'
import { computed } from 'vue'
import { Link } from "@inertiajs/vue3";
import { usePage } from "@inertiajs/vue3";
const props = defineProps({
  href: {
    type: String,
    default: null
  },
  to: {
    type: String,
    default: null
  },
  type: {
    type: String,
    default: 'flex'
  },
  activeColor: {
    type: String,
    default: null
  },
  isDesktopIconOnly: Boolean,
  dropdown: Boolean,
  active: Boolean
})

const itemHref = computed(() => ''
        // props.item.route ? route(props.item.route) : props.item.href
);

const is = computed(() => {
  if (props.href) {
    return 'a'
  }

  // if (props.item.route) {
  //   return Link
  // }

  return 'div'
})
const itemLabel = computed(() =>
        props.item.isCurrentUser ? usePage().props.auth.user.name : props.item.label
);

const styleStore = useStyleStore()

const activeColor = props.activeColor ?? `${styleStore.navBarItemLabelActiveColorStyle} dark:text-slate-400`

const componentClass = computed(() => {
  const base = [
    props.type,
    props.active
      ? activeColor
      : `${styleStore.navBarItemLabelStyle} dark:text-white dark:hover:text-slate-400 ${styleStore.navBarItemLabelHoverStyle}`
  ]

  if (props.type === 'block') {
    base.push('lg:flex')
  }

  if (!props.dropdown) {
    base.push('py-2', 'px-3')
  } else {
    base.push('p-0', 'lg:py-2', 'lg:px-3')
  }

  if (props.isDesktopIconOnly) {
    base.push('lg:w-16', 'lg:justify-center')
  }

  return base
})
</script>

<template>
  <component
    class="items-center grow-0 shrink-0 relative cursor-pointer"
    :is="is"
    :class="componentClass"
    :href="itemHref"
  >
    <slot />
  </component>
</template>
