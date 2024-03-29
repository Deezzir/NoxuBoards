<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import { defineAsyncComponent, defineComponent, markRaw, ref } from 'vue';
import ThemeOption from './ThemeOption.vue';
import { Listbox, ListboxButton, ListboxOptions, ListboxOption } from '@headlessui/vue';
import { LightModeIcon, DarkModeIcon } from '@/components/Icons';
import { ChevronUpDownIcon } from '@heroicons/vue/20/solid';
</script>

<script lang="ts">
export default defineComponent({
    data() {
        return {
            themes: {
                light: {
                    id: 1,
                    name: 'Light',
                    icon: markRaw(defineAsyncComponent(() => import('@/components/Icons/LightModeIcon.vue')))
                },
                dark: {
                    id: 2,
                    name: 'Dark',
                    icon: markRaw(defineAsyncComponent(() => import('@/components/Icons/DarkModeIcon.vue')))
                },
                system: {
                    id: 3,
                    name: 'System',
                    icon: markRaw(defineAsyncComponent(() => import('@/components/Icons/SystemModeIcon.vue')))
                }
            },
            selectedTheme: ref()
        };
    },
    props: {
        isDropdown: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        setTheme(name: string) {
            name = name.toLowerCase();
            localStorage.theme = name;
            if (name === 'system') localStorage.removeItem('theme');
            if (
                localStorage.theme === 'dark' ||
                (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)
            ) {
                document.documentElement.classList.add('dark');
            } else {
                document.documentElement.classList.remove('dark');
            }
        },
        getTheme() {
            if ('theme' in localStorage) {
                if (localStorage.theme === 'dark') return ref(this.themes.dark);
                else return ref(this.themes.light);
            } else {
                return ref(this.themes.system);
            }
        }
    },
    mounted() {
        this.selectedTheme = this.getTheme();
    }
});
</script>

<template>
    <Listbox v-model="selectedTheme" class="relative" as="div">
        <ListboxButton
            class="inline-flex items-center"
            :class="
                isDropdown &&
                'rounded-xl bg-white/[0.85] p-2 shadow-sm ring-1 ring-slate-900/30 dark:bg-slate-900/50 dark:ring-0'
            ">
            <span>
                <LightModeIcon :isActive="!isDropdown" class="inline dark:hidden" />
                <DarkModeIcon :isActive="!isDropdown" class="hidden dark:inline" />
            </span>
            <span
                v-show="isDropdown"
                v-if="selectedTheme"
                class="min-w-[6rem] pl-3 text-2xl text-black dark:text-white">
                {{ selectedTheme.name }}
            </span>
            <span v-show="isDropdown" class="pointer-events-none pl-4">
                <ChevronUpDownIcon class="h-7 w-7 text-black dark:text-gray-300" />
            </span>
        </ListboxButton>

        <Transition
            leave-active-class="transition duration-100 ease-in"
            leave-from-class="opacity-100"
            leave-to-class="opacity-0">
            <ListboxOptions
                class="absolute top-[100%] z-50 overflow-hidden rounded-lg bg-slate-100 py-2 text-lg text-slate-700 shadow-lg ring-1 ring-slate-900/20 dark:bg-slate-700 dark:text-slate-300 dark:ring-0"
                :class="isDropdown ? 'mt-2 w-[12.75rem]' : 'left-[-8px] mt-7 w-[10rem]'">
                <ListboxOption v-for="theme in themes" :key="theme.id" :value="theme" v-slot="{ selected }">
                    <ThemeOption @click="setTheme(theme.name)" :isActive="selected">
                        <template #icon>
                            <component class="mr-2" :isActive="selected" :is="theme.icon" />
                        </template>
                        {{ theme.name }}
                    </ThemeOption>
                </ListboxOption>
            </ListboxOptions>
        </Transition>
    </Listbox>
</template>
