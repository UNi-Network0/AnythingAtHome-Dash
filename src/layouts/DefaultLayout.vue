<script setup>
import { ref } from "vue";
import { useRouter } from 'vue-router';

import {
  PieChartOutlined,
  DesktopOutlined,
  RiseOutlined,
  MenuUnfoldOutlined,
  MenuFoldOutlined,
  HomeFilled,
} from "@ant-design/icons-vue";
const selectedKeys = ref(["1"]);
const collapsed = ref(false);
const router = useRouter()
const toggleCollapsed = () => {
  collapsed.value = !collapsed.value;
};
</script>

<template>
  <a-layout style="min-height: 100vh">
    <a-layout-sider
      v-model:collapsed="collapsed"
      :trigger="null"
      collapsible
      theme="light"
    >
      <div class="logo">
        <HomeFilled v-if="collapsed" class="home-icon" />
        <a-button
          v-if="!collapsed"
          type="link"
          class="logo-button"
          href="#"
          size="large"
        >
          Anything@Home
        </a-button>
      </div>

      <a-menu v-model:selectedKeys="selectedKeys" theme="light" mode="inline">
        <a-menu-item key="1" @click="router.push('/')">
          <PieChartOutlined />
          <span>基础统计</span>
        </a-menu-item>
        <a-menu-item key="2">
          <DesktopOutlined />
          <span>高级统计</span>
        </a-menu-item>
        <a-menu-item key="3" @click="router.push('/rank')">
          <RiseOutlined />
          <span>排行榜</span>
        </a-menu-item>
      </a-menu>
    </a-layout-sider>
    <a-layout>
      <a-layout-header style="background: #fff; padding: 0">
        <menu-unfold-outlined
          v-if="collapsed"
          class="trigger"
          @click="toggleCollapsed"
        />
        <menu-fold-outlined v-else class="trigger" @click="toggleCollapsed" />
      </a-layout-header>
      <a-layout-content :style="{ marginTop: '20px' }">
        <router-view />
      </a-layout-content>
    </a-layout>
  </a-layout>
</template>

<style scoped>
.trigger {
  font-size: 18px;
  line-height: 64px;
  padding: 0 24px;
  cursor: pointer;
  transition: color 0.3s;
}

.trigger:hover {
  color: #1890ff;
}

.logo {
  height: 32px;
  background: rgba(255, 255, 255, 0.3);
  margin: 16px;
}
.home-icon {
  font-size: 24px;
  display: inline-block;
  margin-left: 10px;
}

.logo-button {
  color: inherit;
  padding: 0;
}
</style>
