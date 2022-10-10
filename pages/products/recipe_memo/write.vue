<script setup lang="ts">
import ButtonSecondary from '~~/components/ButtonSecondary.vue';
import RecipeForm from '~~/components/RecipeForm.vue';
import { v4 } from "uuid";
import { RecipeEntity } from "~~/types/RecipeEntity";

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

const dummy = () => {
    alert("工事中");
};

const dummyS = ():string => {
    return "dummy";
};


</script>
<template>
    <PageContainer>
        <div>
            <AppH2>レシピをメモできるアプリ</AppH2>            
            <div class="text-right w-full">
                <ButtonSecondary v-bind:onClick="dummy">
                    アプリTOPに戻る
                </ButtonSecondary>

            </div>
        </div>
        <AppH3>レシピを書く</AppH3>
        <RecipeForm
            v-bind:id="v4"
            redirectOnSuccess="/products/recipe_memo"
            v-model="form">
        </RecipeForm>       
    </PageContainer>
</template>