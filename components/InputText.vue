<script setup lang="ts">
//Vueは双方向バインディングモデル（Reactは単方向モデル）

//親→子への情報伝達用
const props = defineProps<{modelValue: string}>();

//子→親への情報伝達用
const emit = defineEmits(["update:modelValue"]);

//v-modelタグに指定することでバインド。JavaScript変数とフォームの値を連結できる。
//v-modelは、v-dind:modelValueと、@update:modelValueのシンタックスシュガー
const modelValue = computed({
    get: () => props.modelValue,
    set: (value) => emit("update:modelValue", value), //第1引数はイベント名、第2引数は実際に親に渡す値
});
</script>
<template>
    <input 
        v-model="modelValue"
        type="text"
        class="border-2 border-gray-300 rounded-lg p-2 w-full focus:outline-none focus:border-gray-500 focus:shadow-outline-gray"
    />
</template>