<template>
    <div style="position: relative; background: transparent">
        <button class="sort" @click="show = !show">{{ selectedSort }}</button>
        <div v-show="show" class="dropdown">
            <button @click="selectSort('시작일 빠른순')">시작일 빠른순</button>
            <button @click="selectSort('리뷰순')">리뷰순</button>
            <button @click="selectSort('좋아요순')">좋아요순</button>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const emit = defineEmits(['update:sort']);
const show = ref(false);

const selectedSort = ref('시작일 빠른순');

function selectSort(label) {
    selectedSort.value = label;
    show.value = false;

    emit('update:sort', label);
}
</script>

<style scoped>
.sort {
    height: 48px;
    width: 100px;
    word-break: keep-all;
    border: 1px solid #cac4d0;
    border-radius: 10px;
    background-color: #fff;
    line-height: 1;
}

.dropdown {
    padding: 8px 0px;
    border: 1px solid #cac4d0;
    border-radius: 10px;
    background-color: #fff;
    position: absolute;
    top: 110%;
    left: 0;
    overflow: hidden;
    z-index: 1000;
    display: flex;
    flex-direction: column;
    line-height: 1;
    word-break: keep-all;
}

.dropdown button {
    border: none;
    width: 100%;
    height: 40px;
    background-color: #fff;
}
</style>
