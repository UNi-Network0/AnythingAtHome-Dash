<!-- eslint-disable vue/multi-word-component-names -->
<!-- eslint-disable vue/no-unused-vars -->
<template>
    <div class="table-container">
      <a-table :data-source="tableData" row-key="rank">
        <a-table-column title="#" key="rank" width="100">
          <template #default="{ record, index }">
            {{ index + 1 }}
          </template>
        </a-table-column>
        <a-table-column title="节点名称" data-index="clusterName" />
        <a-table-column title="节点请求数" data-index="hits" />
        <a-table-column title="当日流量" data-index="traffic" />
        <a-table-column title="所有者" data-index="ownerName" />
        <a-table-column title="赞助商" key="sponsor">
          <template #default="{ record }">
            <a :href="record.sponsorUrl" target="_blank">{{ record.sponsor }}</a>
          </template>
        </a-table-column>
        <a-table-column title="状态" data-index="status" />
      </a-table>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import { message } from 'ant-design-vue';
  import axios from 'axios';
  
  function formatCommas(num) {
    return num.toLocaleString();
  }
  
  function formatUnits(value) {
    let mbValue = value / 1024 / 1024;
    let gbValue = mbValue / 1024;
    let tbValue = gbValue / 1024;
  
    if (tbValue >= 1) {
      return `${tbValue.toFixed(2)}TB`;
    }
    if (gbValue >= 1) {
      return `${gbValue.toFixed(2)}GB`;
    }
    return `${mbValue.toFixed(2)}MB`;
  }
  
  const tableData = ref([]);
  
  async function fetchData() {
    try {
      const response = await axios.get('https://saltwood.top:9393/93AtHome/rank');
      const data = response.data;
  
      tableData.value = data.map((item, index) => {
        let hits = '0';
        let traffic = '0';
        let status = '';
  
        hits = formatCommas(item.hits || 0);
        traffic = formatUnits(item.traffic || 0);
        status = item.isOnline ? '正常工作' : (item.isBanned ? '被封禁' : '离线');
  
        return {
          rank: index + 1,
          clusterName: item.clusterName || 'null',
          hits,
          traffic,
          status,
          sponsor: item.sponsor || 'null',
          sponsorUrl: item.sponsorUrl || 'null',
          ownerName: item.ownerName || 'null'
        };
      });
    } catch (error) {
        message.error('拉取数据失败：' + error,5);
        console.error('Failed to fetch data:', error);
    }
  }
  
  onMounted(() => {
    fetchData();
  });
  </script>
  
  <style scoped>
  .table-container {
    width: 95%;
    margin: 20px auto;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  </style>
  