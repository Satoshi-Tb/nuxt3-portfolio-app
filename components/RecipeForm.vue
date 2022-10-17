<script setup lang="ts">
import ButtonSecondary from './ButtonSecondary.vue';
import ButtonPrimary from './ButtonPrimary.vue';
import InputTextarea from './InputTextarea.vue';

const props = defineProps<{
    modelValue: {
        name: string;
        items: {
            name: string;
            amount: number | null;
            unit: string;
        }[];
        howToCook: string;
    },
    id: string;
    redirectOnSuccess: string;
}>();
const emit = defineEmits(["update:modelValue"]);
//computedの結果へのアクセスについて
//scriptタグ内では、valueプロパティ経由。（form.value.xxx）
//templateタグ内では、直接。（form.xxx）
const form = computed({
    get: () => props.modelValue,
    set: (value) => emit("update:modelValue", value),
});
const removeItem = (index: number) => {
    form.value.items.splice(index, 1);
};
const addItem = () => {
    form.value.items.push({
        name: "",
        amount: null,
        unit: "",
    });
};
const dbName = "recipe-memo";
const tableName = "recipe";
const submit = () => {
    const {name, items, howToCook: howToCook} = form.value;

    if (!name 
        || !howToCook 
        || items.some(item => !item.name || !Number.isFinite(item.amount) || !item.unit)) {
        alert("いずれかのフォームが空白です");
        return;
    }

    const openRequest = indexedDB.open(dbName);
    openRequest.onsuccess = (e) => {
        //IndexedDBについて
        //通常、この機能は伝統的なクライアント-サーバアプリケーションには過大です。
        //IndexedDB は、ServiceWorkers や他のテクノロジーと組み合わせるオフラインアプリケーションを想定しています。
        
        //as:TypeScriptの型アサーション機能。
        //キャストに近いと思う。任意の型を指定できるというものではない
        const db = (e.target as IDBRequest).result;
        const transaction = db.transaction(tableName, "readwrite");
        const table = transaction.objectStore(tableName) as IDBObjectStore;
        const id = props.id;
        const putRequest = table.put({
            id, 
            name,
            //TODO
            //ここの配列化理由がわからず。シリアライズ可能な状態にすることが目的とのこと。
            //もともとオブジェクトの配列のため、シリアライズ可能と思ったが違うの？
            items:items.map(item => ({
                name: item.name,
                amount: item.amount,
                unit: item.unit
            })),
            howToCook: howToCook
        });
        putRequest.onsuccess = () => {
            alert("保存に成功しました");
            return navigateTo(props.redirectOnSuccess);
        };
        putRequest.onerror = () => {
            alert("保存に失敗しました");
        };
    }
};

//todo debug
onMounted(() => {
//    console.log("RecipeForm mounted");
//    console.log(props);
});

</script>
<template>
    <div class="grid gap-8">
        <label>レシピの名前
            <InputText v-model="form.name" />
        </label>
        <div
            v-for="(item, index) in form.items"
            class="bg-cyan-100 p-4 rounded-md shadow-md"
        >
            <div class="w-full text-right">
                <ButtonSecondary v-bind:onClick="() => removeItem(index)">材料{{index + 1}}を削除する</ButtonSecondary>
            </div>
            <div class="grid gap-4 sm:grid-cols-3">
                <label>
                    材料 {{index + 1}} の名前
                    <InputText v-model="item.name" />
                </label>
                <label>
                    材料 {{index + 1}} の個数
                    <InputNumber v-model="item.amount" />
                </label>
                <label>
                    材料 {{index + 1}} の単位
                    <InputText v-model="item.unit" />
                </label>
            </div>
        </div>
        <div>
            <ButtonPrimary v-bind:onClick="addItem">
                材料を追加する
            </ButtonPrimary>
        </div>
        <label>
            作り方
            <InputTextarea v-model="form.howToCook"></InputTextarea>
        </label>
        <div class="w-full text-right">
            <ButtonPrimary v-bind:onClick="submit">
                レシピを保存する
            </ButtonPrimary>
        </div>
    </div>
</template>