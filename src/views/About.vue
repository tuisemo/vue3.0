<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button ref="dom" @click="addList">添加</button>
    <button @click="reactData.setCount">自加</button>
    <p>{{ length }}条数据</p>
    <p>{{ reactData.count }}静态</p>
    <ul>
      <li
        :key="item.id"
        v-for="(item, i) in list"
        :ref="
          (el) => {
            if (el) domList[i] = el;
          }
        "
      >
        {{ item.value }}
      </li>
    </ul>
  </div>
</template>
<script>
import { ref, watch, reactive, onMounted, computed, watchEffect } from "vue";
export default {
  setup() {
    const list = ref([]);
    const reactData = reactive({
      count: 0,
      setCount: () => {
        reactData.count++;
      },
    });
    const dom = ref(null);
    const domList = ref([]);
    const addList = () => {
      list.value.push({
        id: new Date().getTime(),
        value: `${new Date().getTime()}`,
      });
    };
    onMounted(() => {
      console.log("组件已挂载");
      console.log(dom.value);
    });
    watch(list.value, (newValue, oldValue) => {
      console.log(
        "🚀 ~ file: About.vue ~ line 25 ~ watch ~ oldValue",
        oldValue
      );
      console.log(
        "🚀 ~ file: About.vue ~ line 25 ~ watch ~ newValue",
        newValue
      );
    });
    watchEffect(() => {
      console.log("---------------", list.value);
      console.log("---------------", reactData.count);
    });
    const length = computed(() => {
      return list.value.length;
    });
    return {
      dom,
      reactData,
      domList,
      list,
      addList,
      length,
    };
  },
};
</script>
