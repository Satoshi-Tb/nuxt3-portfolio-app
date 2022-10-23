<script setup lang="ts">
//refは状態を定義するメソッド
//reactiveと似ている。
//reactiveとの違い
//reactiveは常にvalue経由でアクセスに対して、
//refはscript内ではvalue経由、template内では直接アクセス
//refはプリミティブな値を状態として定義できるが、reactiveはオブジェクトのみ
//全部refでも構わないが、全部reactiveは避けたほうが良い（プリミティブ型利用できないので）
const links = ref<
  {
    url: string;
    text: string;
  }[]
>([]);

const dbName = "recipe-memo";
const tableName = "recipe";

onMounted(() => {
  const openRequest = indexedDB.open(dbName);

  openRequest.onsuccess = (evt) => {
    const db = (evt.target as IDBRequest).result;
    const transaction = db.transaction(tableName, "readonly");
    const table = transaction.objectStore(tableName) as IDBObjectStore;

    const cursorRequest = table.openCursor();

    cursorRequest.onsuccess = (evt) => {
      const cursor = (evt.target as IDBRequest).result;

      //最後のレコードまで操作した場合、カーソルがnullになっているはず
      if (!cursor) return;

      const record = cursor.value;

      links.value.push({
        url: `/products/recipe_memo/${record.id}`,
        text: record.name,
      });

      //次のカーソルに進める
      //次のカーソルに進んだら、またonsuccessがコールバックされる
      cursor.continue();
    };
  };

  //アプリ内でこのページを最初に開くユースケースが中心のはずなので
  //ここでもテーブル作成処理を挟んでおく
  openRequest.onupgradeneeded = (evt) => {
    const db = (evt.target as IDBRequest).result;
    db.createObjectStore(tableName, { keyPath: "id" });
  };
});

const goBack = () => {
  //ポートフォリオサイトのトップに移動
  return navigateTo("/");
};

const goWrite = () => {
  return navigateTo("/products/recipe_memo/write");
};
</script>
<template>
  <PageContainer>
    <div>
      <AppH2>レシピをメモできるアプリ</AppH2>
      <div class="text-right w-full">
        <ButtonSecondary v-bind:onClick="goBack">
          サイトTOPに戻る
        </ButtonSecondary>
      </div>
    </div>
    <AppH3>レシピ一覧</AppH3>
    <AppLink v-bind:links="links" />
    <ButtonIconPlus v-bind:onClick="goWrite" />
  </PageContainer>
</template>
