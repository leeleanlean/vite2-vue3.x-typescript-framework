<template>
  <div
    v-if="config.table"
    class="table-content bg-ffffff p-16"
    :style="{
      marginBottom: config?.footer?.fixed ? '62px' : '0',
    }"
  >
    <a-table
      :data-source="config.table.data"
      :columns="config.table.columns"
      :row-selection="
        config?.table?.selectedRows
          ? {
              type: config.table?.selectedType ?? 'checkbox',
              selectedRowKeys: selectedRowKeys(),
              onChange: onChange,
            }
          : null
      "
      :row-key="rowKey"
      :pagination="false"
      v-bind="config.table.others"
    >
      <!-- 内容 -->
      <template #bodyCell="{column, text, record, index}">
        <!-- 自定义插槽 -->
        <template v-if="column?.type === 'slot'">
          <slot :name="column?.dataIndex ?? ''" :rows="record"></slot>
        </template>
        <!-- 表格列 -->
        <template v-else>
          <!-- 序号 -->
          <template v-if="['#', '序号'].includes(column.title)">{{ index + 1 }}</template>
          <!-- 默认列 -->
          <template v-else>{{ text }}</template>
        </template>
      </template>
      <!-- 底部 -->
      <template v-if="config.table.footer" #footer>
        <span class="color-info">{{ config.table.footer }}</span>
      </template>
    </a-table>
  </div>
</template>
<script setup lang="ts">
const props = defineProps<{
  config: any;
}>();
const emit = defineEmits(["update:config"]);

const rowKey = (record: any) => record.id;
const selectedRowKeys = () => props.config.table.selectedRows.map((item: any) => item.id);
const onChange = (selectedRowKeys: any, selectedRows: any) => {
  let DATA = {
    ...props.config,
    table: {
      ...props.config.table,
      selectedRows,
    },
  };
  props.config.footer &&
    (DATA = {
      ...DATA,
      footer: {
        ...props.config.footer,
        checked: selectedRows.length === props.config.table.data.length,
      },
    });
  emit("update:config", DATA);
};
</script>
<style lang="scss">
.table-content .ant-table-cell a.link {
  margin-right: 12px;
  color: #1a73e8;
  font-size: 13px;
  &:last-child {
    margin-right: 0;
  }
}
</style>
