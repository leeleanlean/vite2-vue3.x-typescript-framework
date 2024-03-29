<template>
  <div class="l-table">
    <TableSearch v-model:config="deepConfig" @update:config="updateConfig">
      <!-- 自定义插槽 -->
      <template v-for="slot in slots" :key="slot" #[slot]="rows">
        <slot :name="slot" :rows="rows.rows"></slot>
      </template>
    </TableSearch>
    <a-spin :spinning="loading">
      <!-- 工具栏 -->
      <TableToolbar v-model:config="deepConfig" @update:config="updateConfig" @click:operate="clickOperate">
        <template v-if="toolbarOperates" #toolbar-operates>
          <slot name="toolbar-operates"></slot>
        </template>
      </TableToolbar>
      <!-- 自定义头部插槽 -->
      <div v-if="customHeader" class="bg-ffffff p-l16 p-t16">
        <slot name="custom-header"></slot>
      </div>
      <TableContent v-model:config="deepConfig" @update:config="updateConfig">
        <!-- 自定义插槽 -->
        <template v-for="slot in slots" :key="slot" #[slot]="rows">
          <slot :name="slot" :rows="rows.rows"></slot>
        </template>
      </TableContent>
      <TableFooter
        v-model:config="deepConfig"
        @update:config="updateConfig"
        @click:operate="clickOperate"
      ></TableFooter>
    </a-spin>
  </div>
</template>
<script setup lang="ts">
import {onMounted, ref, useSlots} from "vue";
import TableSearch from "./table-search.vue";
import TableToolbar from "./table-toolbar.vue";
import TableContent from "./table-content.vue";
import TableFooter from "./table-footer.vue";

const props = defineProps<{
  config: any;
}>();
const emit = defineEmits(["click:operate", "update:config"]);
const deepConfig = ref(props.config);

const slots = Object.keys(useSlots());
const customHeader = !!useSlots()["custom-header"];
const toolbarOperates = !!useSlots()["toolbar-operates"];

/**
 * 请求数据
 */
const loading = ref(false);
const getData = async () => {
  loading.value = true;
  const res = await props.config.request({
    ...(deepConfig.value?.search?.data ?? {}),
    page: deepConfig.value?.footer?.pagination?.currentPage ?? 1,
    pageSize: deepConfig.value?.footer?.pagination?.pageSize ?? 10,
  });
  deepConfig.value.table.data = res.data;
  deepConfig.value.footer && (deepConfig.value.footer.pagination.total = res.total);
  deepConfig.value.footer && (deepConfig.value.footer.checked = false);
  loading.value = false;
};

/**
 * 更新数据
 */
const updateConfig = (v: any, refresh: boolean) => {
  deepConfig.value = v;
  emit("update:config", deepConfig.value);
  refresh && props.config.request && getData();
};

/**
 * 操作按钮
 */
const clickOperate = (item: any, data: any) => emit("click:operate", item, data);

/**
 * 默认请求数据
 */
onMounted(() => props.config.request && getData());
</script>

<style lang="scss" scoped>
.l-table {
  :deep(.ant-spin-nested-loading > div > .ant-spin) {
    background-color: rgba(255, 255, 255, 0.5);
  }
}
</style>
