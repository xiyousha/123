<template>
  <div
    class="infinite-list"
    style="overflow: auto"
    ref="list"
  >
    <div
      class="item"
      v-for="(item, index) in data.list"
      :key="index"
      :style="{ background: item.background }"
    >
      {{ index }}
    </div>
    <div v-if="loading && data.list.length < 50">Loading...</div>
    <div v-if="noMore">No more</div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref, computed, onMounted } from "vue";

const loading = ref(false);
const list = ref();

const data = reactive({
  list: [{ background: "rgb(233,32,38)" }],
  height: 0,
});

const noMore = computed(() => data.list.length >= 50);
const disabled = computed(() => loading.value || noMore.value);

const getList = (num: number) => {
  for (let i = 0; i < num; i++) {
    let bg = "";
    bg =
      "#" +
      Math.floor(Math.random() * 255).toString(16) +
      Math.floor(Math.random() * 255).toString(16) +
      Math.floor(Math.random() * 255).toString(16);
    data.list.push({ background: bg });
  }
  loading.value = false;
};

const load = () => {
  loading.value = true;

  if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
  console.log(123)

    if (data.list.length < 50) {
      setTimeout(() => {
        getList(10);
      }, 500);
    }
  }
};

onMounted(() => {
  getList(10);
  data.height = window.screen.height;
  window.addEventListener("scroll", load);
});
</script>

<style scoped>
.infinite-list {
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
