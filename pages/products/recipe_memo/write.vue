<script setup lang="ts">
import ButtonSecondary from '~~/components/ButtonSecondary.vue';
import RecipeForm from '~~/components/RecipeForm.vue';
import { v4 } from "uuid";
import { RecipeEntity } from "~~/types/RecipeEntity";

//reactiveで値が変わるごとに再描画する「状態」を定義する
const form = reactive<RecipeEntity>({
    name: "",
    items: [
        {
            name: "",
            amount: null as null | number,
            unit: "",
        }
    ],
    howToCook: "",
});

const dbName = "recipe-memo";
const tableName = "recipe";
//mountedライフサイクルフック
//サーバで初期化を終えたコンポーネントが、ブラウザ上に組み込まれたタイミングで実施される
onMounted(() => {
    //IndexedDBにテーブルを作成
    const openRequest = indexedDB.open(dbName);
    openRequest.onupgradeneeded = (evt) => {
        const db = (evt.target as IDBRequest).result;
        db.createObjectStore(tableName, {keyPath: "id"});
    };
});

const goBack = () => {
    return navigateTo("/products/recipe_memo");
}

</script>
<template>
    <PageContainer>
        <div>
            <AppH2>レシピをメモできるアプリ</AppH2>            
            <div class="text-right w-full">
                <ButtonSecondary v-bind:onClick="goBack">
                    アプリTOPに戻る
                </ButtonSecondary>
            </div>
        </div>
        <AppH3>レシピを書く</AppH3>
        <RecipeForm
            v-bind:id="v4()"
            redirectOnSuccess="/products/recipe_memo"
            v-model="form">
        </RecipeForm>       
    </PageContainer>
</template>