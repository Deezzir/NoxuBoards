<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    props: {
        isActive: Boolean
    }
});
</script>

<template>
    <svg viewBox="0 0 24 24" fill="none" class="h-12 w-12 sm:h-10 sm:w-10">
        <path
            d="M4 6a2 2 0 0 1 2-2h12a2 2 0 0 1 2 2v7a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V6Z"
            stroke-width="2"
            stroke-linejoin="round"
            :class="isActive ? 'fill-sky-400/20 stroke-sky-500' : 'stroke-slate-400 dark:stroke-slate-500'"></path>
        <path
            d="M14 15c0 3 2 5 2 5H8s2-2 2-5"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            :class="isActive ? 'stroke-sky-500' : 'stroke-slate-400 dark:stroke-slate-500'"></path>
    </svg>
</template>
