<script setup lang="ts">
import {ref, computed, watch, onMounted} from 'vue';
import CarTypeFilter from '@/components/CarTypeFilter.vue'
import MobileScroll from '@/components/MobileScroll.vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

const isMobile = ref(false)



const formData = computed(() => route.query)

const page = ref(1)
const size = ref(10)
const totalData =ref(new Array(1000).fill(0).map((v,index)=>index+1))

const maxPage = computed(() => {
  return totalData.value.length % size.value ===0 ? totalData.value.length / size.value: (totalData.value.length / size.value + 1);
})

const tableData = computed(() => {
  if(totalData.value.length===0) {
    return []
  }
  return totalData.value.slice((page.value-1)*size.value, page.value*size.value)
})

const handlePrev = () => {
  if(page.value>1) {
    page.value -= 1
  }
}

const handleNext = () => {
  if(page.value <maxPage.value) {
    page.value+= 1
  }
}

const jump = (value) => {
  page.value = value
}

const paginationList = computed(() => {
  const siblingCount = 1
  const totalNumbers = 1 * 2 + 5; // 当前页、前后、首尾、2个省略
  const pagination = [];

  if (maxPage.value <= totalNumbers) {
    // 情况1：页数少，全部显示
    for (let i = 1; i <= totalPages; i++) {
      pagination.push(i);
    }
  } else {
    const leftSibling = Math.max(page.value - siblingCount, 2);
    const rightSibling = Math.min(page.value + siblingCount, maxPage.value - 1);

    const showLeftDots = leftSibling > 2;
    const showRightDots = rightSibling < maxPage.value - 1;

    pagination.push(1); // 首页



    for (let i = leftSibling; i <= rightSibling; i++) {
      pagination.push(i);
    }



    pagination.push(maxPage.value); // 尾页
  }

  return pagination;
})


watch(formData, (newValue) => {

})

onMounted(() => {
  const mediaQuery = window.matchMedia('(max-width: 768px)');
  isMobile.value = mediaQuery.matches;
})

</script>

<template>
  <main>
    <div class="filters">
      <CarTypeFilter v-if="!isMobile"/>
    </div>
    <MobileScroll v-if="isMobile" />
    <div class="container" v-else>
      <div class="car" v-for="car in tableData" :key="car">{{car}}</div>
    </div>
    <div class="pagination">
      <div @click="jump(item)" :class="{ page: true, selected: page == item }" v-for="item in paginationList">{{item}}</div>
    </div>
  </main>
</template>
<style lang="css" scoped>
.pagination{ 
  display: flex;
  position: absolute;
  bottom: 20px;
}
.container {
  
}

.car {
  display: inline-block;
  width: 20%;
  padding-top: 15%;
}

.page {
    background: grey;
    display: inline-flex;
    align-items: center;
    color: #fff;
    margin-right: 10px;
    padding-left: 10px;
    padding-right: 10px;
    margin-bottom: 10px;
}

.page.selected {
    background: lightGreen;
}
</style>
