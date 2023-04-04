<template>
  <div
    class="infinite-list"
    style="overflow: auto" 
    v-infinite-scroll="load"
    infinite-scroll-distance="50"
    :infinite-scroll-disabled="disabled"
  >
    <div class="item" v-for="(item, index) in data.list" :key="index" :style="{ 'background': item.background }">{{ index }}</div>
    <div v-if="loading">Loading...</div>
    <div v-if="noMore">No more</div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref, computed } from "vue";
const loading = ref(false)

const data = reactive({
  list: [{ background: "rgb(233,32,38)" }],
});

const noMore = computed(() => data.list.length >= 50)
const disabled = computed(() => loading.value || noMore.value)
const getList = (num: number) => {
  for (let i = 0; i < num; i++) {
    let bg = ''
    bg = "#" +
          Math.floor(Math.random() * 255).toString(16) +
          Math.floor(Math.random() * 255).toString(16) +
          Math.floor(Math.random() * 255).toString(16);
    data.list.push({ background: bg })
  }
  loading.value = false
}

const load = () => {
    loading.value = true
    setTimeout(() => {
      getList(10)
    }, 500)
};
</script>

<style scoped>

.infinite-list {
  height: 100vh;
  padding: 0;
  margin: 0;
  list-style: none;
  text-align: center;
}
.item {
    width: 100%;
    height: 200px;
    display: flex;
    align-items: center;
    justify-content: center;
}
.infinite-list .item + .list-item {
  margin-top: 10px;
}
</style>
