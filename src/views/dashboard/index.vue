<template>
  <div class="dashboard">
    <template v-if="loading">
      <a-skeleton-button active block size="large" />
      <a-row type="flex" :gutter="16">
        <a-col flex="auto">
          <a-skeleton-button active block size="large" class="m-t16" />
          <a-skeleton-button active block size="large" class="m-t16" />
          <a-skeleton-button active block size="large" class="m-t16" />
        </a-col>
        <a-col flex="0 1 460px">
          <a-skeleton-button active block size="large" class="m-t16" />
          <a-skeleton-button active block size="large" class="m-t16" />
          <a-skeleton-button active block size="large" class="m-t16" />
        </a-col>
      </a-row>
    </template>
    <template v-else>
      <a-card title="数据概览" :bordered="false">
        <a-row type="flex">
          <a-col class="p-l56 p-tb8" :span="6">
            <a-statistic title="Active Users" :value="112893" style="margin-right: 50px" />
          </a-col>
          <a-col class="p-l56 p-tb8" :span="6">
            <a-statistic title="Account Balance (CNY)" :precision="2" :value="112893" />
          </a-col>
          <a-col class="p-l56 p-tb8" :span="6">
            <a-statistic title="Active Users" :value="112893" style="margin-right: 50px" />
          </a-col>
          <a-col class="p-l56 p-tb8" :span="6">
            <a-statistic title="Account Balance (CNY)" :precision="2" :value="112893" />
          </a-col>
        </a-row>
      </a-card>
      <a-row type="flex" :gutter="16">
        <a-col flex="auto" class="m-t16">
          <a-card title="散点图" :bordered="false">
            <chart1></chart1>
          </a-card>
          <div class="m-t16">
            <a-card title="折线图" :bordered="false">
              <chart2></chart2>
            </a-card>
          </div>
        </a-col>
        <a-col flex="400px" class="m-t16">
          <div class="bg-ffffff">
            <a-card title="进度状态" :bordered="false">
              <a-progress class="m-b8" :percent="30" />
              <a-progress class="m-b8" :percent="50" status="active" />
              <a-progress class="m-b8" :percent="70" status="exception" />
              <a-progress class="m-b8" :percent="100" />
              <a-progress :percent="50" :show-info="false" />
            </a-card>
          </div>
          <div class="m-t16">
            <a-card title="流程状态" :bordered="false">
              <a-timeline pending="Recording...">
                <a-timeline-item>Create a services site 2015-09-01</a-timeline-item>
                <a-timeline-item>Solve initial network problems 2015-09-01</a-timeline-item>
                <a-timeline-item>Technical testing 2015-09-01</a-timeline-item>
              </a-timeline>
            </a-card>
          </div>
        </a-col>
      </a-row>
    </template>
  </div>
</template>
<script setup lang="ts">
import {ref} from "vue";
import Chart1 from "./chart1.vue";
import Chart2 from "./chart2.vue";

const loading = ref(true);
setTimeout(() => (loading.value = false), 600);
</script>

<style scoped>
.dashboard :deep(.ant-skeleton-element .ant-skeleton-button) {
  height: 56px;
}
</style>
