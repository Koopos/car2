<script setup>
import { onMounted, onUnmounted } from 'vue'

const handleScroll = () => {
  const scrollTop = window.pageYOffset || document.documentElement.scrollTop
  const clientHeight = document.documentElement.clientHeight
  const scrollHeight = document.documentElement.scrollHeight

  if (scrollTop + clientHeight >= scrollHeight - 10) {
    loadMore()
  }
}

const data =ref(new Array(100).fill(0))

const page = ref(1)

const getData = () => {
    data.value = [...data.value, new Array(10).fill(0)]
}

const loadMore = () => {
    page.value += 1
    getData()
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>