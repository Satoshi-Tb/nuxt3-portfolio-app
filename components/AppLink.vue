<script setup lang="ts">
// const props = defineProps<{
//     links: {
//         url: string;
//         text: string;
//     }[]
// }>();

const props = defineProps<{
    links: Array<{
        url: string;
        text: string;
    }>
}>();

</script>
<template>
    <ul class="p-8 grid gap-y-4 rounded-md bg-slate-200">
        <li v-for="link in props.links">
            <NuxtLink v-bind:href="link.url" class="text-blue-700 hover:opacity-70">
                {{ link.text }}
            </NuxtLink>
        </li>
        <slot />
    </ul>
</template>