<script setup lang="ts">
import {ref, computed,watch} from 'vue';
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

const isToggle = ref(true)

const carTypeList = ref(new Array(20).fill(0).map((v, index)=>({
    label: 'b'+index,
    value: index
})))

const selectedValue = ref(carTypeList?.[0]?.value)


const handleSelect = (val) => {
    selectedValue.value = val
}

const selectedItem = computed(() => {
    return carTypeList.value.find(v=>v.value === selectedValue.value)?.label
})

watch(selectedValue, (newValue) => {
    router.replace({
        path: route.path,
        query: {
            ...route.query,
            carType: newValue
        }
    })
},{
    immediate: true
})

</script>

<template>
<div>
车型：
<div class="container">
    <div :class="{item: true, selected: item.value === selectedValue}" @click="handleSelect(item.value)" v-for="item in carTypeList.slice(0,7)" :key="item.value">{{item.label}}</div>
    <div :class="{item: true, selected: item.value === selectedValue}" @click="handleSelect(item.value)" v-for="item in carTypeList.slice(7)" :key="item.value" v-if="!isToggle">{{item.label}}</div>
    <span @click="isToggle=!isToggle">{{ isToggle? '展开': '收起'}}</span>
</div>

</div>
</template>
<style lang="css" scoped>

.item {
    background: grey;
    display: inline-flex;
    align-items: center;
    color: #fff;
    margin-right: 10px;
    padding-left: 10px;
    padding-right: 10px;
    margin-bottom: 10px;
    cursor:pointer;
}

.item.selected {
    background: lightGreen;
}
.container {
    width: 600px;
}
</style>