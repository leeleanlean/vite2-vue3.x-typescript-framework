<template>
  <div v-if="config?.search?.forms?.length" class="table-search bg-ffffff m-b16 p-t24 p-r24">
    <a-form ref="formRef" name="l-table-search" :model="formState">
      <a-row :gutter="24">
        <a-col
          v-for="item in config.search.forms"
          :key="item.label"
          v-bind="config?.search?.col ? config.search.col : {span: 6}"
          style="padding-left: 36px"
        >
          <!-- input -->
          <a-form-item
            v-if="!item.type || item.type === 'input'"
            :label="item.label"
            :name="item.name"
            :rules="item.rules"
          >
            <a-input
              v-model:value="formState[item.name]"
              :allow-clear="true"
              :placeholder="`请输入${item.label}`"
              v-bind="item.others"
              @change="onChange"
            />
          </a-form-item>
          <!-- date -->
          <a-form-item v-if="item.type === 'date'" :label="item.label" :name="item.name" :rules="item.rules">
            <a-date-picker
              v-model:value="formState[item.name]"
              :placeholder="`请输入${item.label}`"
              style="width: 100%"
              v-bind="item.others"
              @change="onChange"
            />
          </a-form-item>
          <!-- daterange -->
          <a-form-item v-if="item.type === 'daterange'" :label="item.label" :name="item.name" :rules="item.rules">
            <a-range-picker
              v-model:value="formState[item.name]"
              :placeholder="[`开始${item.label}`, `结束${item.label}`]"
              style="width: 100%"
              v-bind="item.others"
              @change="onChange"
            />
          </a-form-item>
          <!-- select -->
          <a-form-item v-if="item.type === 'select'" :label="item.label" :name="item.name" :rules="item.rules">
            <a-select
              v-model:value="formState[item.name]"
              :allow-clear="true"
              :placeholder="`请选择${item.label}`"
              v-bind="item.others"
              @change="onChange"
            >
              <a-select-option v-for="option in item.options" :key="option.label" :value="option.value">
                {{ option.label }}
              </a-select-option>
            </a-select>
          </a-form-item>
          <!-- select -->
          <a-form-item v-if="item.type === 'slot'" :label="item.label" :name="item.name" :rules="item.rules">
            <slot :name="item.name"></slot>
          </a-form-item>
        </a-col>
        <!-- <a-col flex="auto" class="m-b24" style="text-align: right">
          <a-button type="primary" html-type="submit">查询</a-button>
          <a-button class="m-lr8" @click="() => formRef.resetFields()">重置</a-button>
          <a-button type="text">折叠/展开</a-button>
        </a-col> -->
      </a-row>
    </a-form>
  </div>
</template>
<script setup lang="ts">
import {ref, reactive} from "vue";
import type {FormInstance} from "ant-design-vue";
import _ from "lodash";

const props = defineProps<{
  config: any;
}>();
const emit = defineEmits(["update:config"]);

const formRef: any = ref<FormInstance>();
const formState: any = reactive({});

/**
 * 表格搜索
 */
const onChange = _.debounce(() => {
  let DATA = {
    ...props.config,
    search: {
      ...props.config.search,
      data: {
        ...props.config.search.data,
        ...formState,
      },
    },
  };
  props.config.footer &&
    (DATA = {
      ...DATA,
      footer: {
        ...props.config.footer,
        pagination: {
          ...props.config.footer.pagination,
          currentPage: 1,
        },
      },
    });
  emit("update:config", DATA, true);
}, 600);
</script>
<style lang="scss" scoped>
.table-search {
  :deep(.ant-form-horizontal .ant-form-item-label) {
    width: 90px !important;
    text-align: right !important;
  }
  :deep(.ant-form-item-label > label) {
    font-size: 13px !important;
  }
}
</style>
