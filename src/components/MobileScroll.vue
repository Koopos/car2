<script setup>
import { onMounted, onUnmounted, ref } from 'vue'

const data =ref(new Array(100).fill(0))

const page = ref(1)

const scrollRef = ref(null)

const handleScroll = () => {
  const scrollTop = scrollRef.value.scrollTop
  const clientHeight = scrollRef.value.clientHeight
  const scrollHeight = scrollRef.value.scrollHeight
  if (scrollTop + clientHeight >= scrollHeight - 10) {
    loadMore()
  }
}

const getData = () => {
    data.value = [...data.value, ...new Array(10).fill(0)]
}

const loadMore = () => {
    page.value += 1
    getData()
}

onMounted(() => {
    scrollRef.value.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
 scrollRef.value.removeEventListener('scroll', handleScroll)
})
</script>
<template>
<div class="scroll-container" ref="scrollRef">
    <div class="car-item" v-for="item in data" :key="item">{{item}}</div>
</div>
</template>
<style lang="css">
.car-item {
    width: 100%;
    padding-top: 67%;
}
.scroll-container {
    height: 800px;
    overflow-y: scroll;
}
</style>