<template>
    <PageContainer>
        <AppH2>このサイトについて</AppH2>
        <div>
            <p>このサイトは、ポートフォリオサイトです。</p>
            <p>Nuxt3で構成しており、CSSフレームワークにはTailwindCSSを使っています。</p>
        </div>
    </PageContainer>
</template>
<script>
import PageContainer from '~~/components/PageContainer.vue';
import AppH2 from '~~/components/AppH2.vue';

export default {
    components: { PageContainer, AppH2 }
};
</script>