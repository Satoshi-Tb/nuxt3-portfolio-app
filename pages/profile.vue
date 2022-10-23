<template>
  <PageContainer>
    <AppH2>プロフィール</AppH2>
    <div class="sm:inline-flex">
      <img class="max-h-40" src="/img/my-icon.png" />
      <div class="grid gap-y-4 sm:ml-4">
        <div>
          <AppH3>名前</AppH3>
          <div>わたしだ</div>
        </div>
        <div>
          <AppH3>経歴</AppH3>
          <div>
            <ul>
              <li>xxxx年yy月 ○○大学卒業</li>
              <li>xxxx年yy月 ○○テクノロジー社入社</li>
              <li>xxxx年yy月 ××社入社</li>
            </ul>
          </div>
        </div>
        <div>
          <AppH3>趣味</AppH3>
          <div>
            <p>登山・キャンプ</p>
          </div>
        </div>
      </div>
    </div>
  </PageContainer>
</template>
<script>
import PageContainer from "~~/components/PageContainer.vue";
import AppH2 from "~~/components/AppH2.vue";
import AppH3 from "~~/components/AppH3.vue";

export default {
  components: { PageContainer, AppH2, AppH3 },
};
</script>
