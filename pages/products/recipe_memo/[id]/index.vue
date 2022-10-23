<script setup lang="ts">
import { RecipeEntity } from "~~/types/RecipeEntity";

//URLに関する情報が格納されたルートオブジェクト
const route = useRoute();
//動的ルート[id]にあたるURLの文字列を取得する
const id = route.params.id;

const dbName = "recipe-memo";
const tableName = "recipe";
const record = ref<RecipeEntity | null>(null);

onMounted(() => {
  const openRequest = indexedDB.open(dbName);

  openRequest.onsuccess = (evt) => {
    const db = (evt.target as IDBRequest).result;
    const transaction = db.transaction(tableName, "readonly");
    const table = transaction.objectStore(tableName) as IDBObjectStore;

    const getRequest = table.get(id);

    getRequest.onsuccess = (evt) => {
      record.value = (evt.target as IDBRequest).result;

      if (!record.value) {
        alert("レシピが見つかりません。");
        return navigateTo("/products/recipe_memo");
      }
    };
  };
});
</script>
<template>
  <PageContainer>
    <div>
      <AppH2>{{ record ? `${record.name}のレシピ` : "読み込み中…" }}</AppH2>
      <div class="text-right w-full">
        <ButtonSecondary v-bind:onClick="() => {}">
          アプリTOPに戻る
        </ButtonSecondary>
      </div>
    </div>
    <AppH3>材料</AppH3>
    <div>
      <div v-for="(item, index) in record?.items">
        {{ item.name }} {{ item.amount }} {{ item.unit }}
      </div>
    </div>
    <AppH3>作り方</AppH3>
    <div class="whitespace-pre-wrap">
      {{ record?.howToCook }}
    </div>
    <div v-if="record" class="grid sm:grid-cols-2 gap-3">
      <ButtonPrimary v-bind:onClick="() => {}">
        このレシピを編集する
      </ButtonPrimary>
      <ButtonPrimary v-bind:onClick="() => {}">
        このレシピを削除する
      </ButtonPrimary>
    </div>
  </PageContainer>
</template>
