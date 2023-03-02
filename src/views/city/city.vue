<template>
  <div class="city top-page">
    <div class="top">
      <van-search
        v-model="searchValue"
        placeholder="城市/区域/位置"
        shape="round"
        show-action
        @cancel="cancelClick"
      />
      <van-tabs v-model:active="tableActive" color="#ff9856">
        <template v-for="(value, key, index) in allCities" :key="key">
          <van-tab :title="value.title" :name="key"></van-tab>
        </template>
      </van-tabs>
    </div>
    <div class="content">
      <template v-for="(value, key, index) in allCities">
        <city-group
          v-show="tableActive === key"
          :group-data="value"
        ></city-group>
      </template>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { useRouter } from "vue-router";
import useCityStore from "@/stores/modules/city";
import { storeToRefs } from "pinia";

import CityGroup from "./cpns/city-group.vue";

const router = useRouter();

// 搜索框功能
const searchValue = ref("");
const cancelClick = () => {
  router.back();
};

// tab的切换
const tableActive = ref(0);

// 从Store中获取数据
const cityStore = useCityStore();
cityStore.fetchAllCitiesData();
const { allCities } = storeToRefs(cityStore);

// 获取选中标签后的数据
// 1. 获取正确的key：将tabs中绑定的tabAciton正确绑定
// 2. 根据key从allCities获取数据，默认直接获取的数据不是响应式的
const currentGroup = computed(() => allCities.value[tableActive.value]);
</script>

<style lang="less" scoped>
.city {
  .content {
    height: calc(100vh - 98px);
    overflow-y: auto;
  }
}
</style>
