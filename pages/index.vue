<script setup lang="ts">
const links=[{
    url:'/about',
    text:'このサイトについて'
}, {
    url:'/profile',
    text:'プロフィール'
}, {
    url:'/products',
    text:'作品集'
}];
</script>
<template>
    <PageContainer>
        <ApppH2>トップページ</ApppH2>
        <AppLink v-bind:links="links" />
    </PageContainer>
</template>