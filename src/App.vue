<template>
  <a-config-provider
      :theme="{
        token: {
          fontFamily: `HMFont, sans-serif`,
          fontSize: 15,
          lineWidth: 0,
          fontWeightStrong: 900
        }
      }"
    >
  <DefaultLayout>
  </DefaultLayout>
</a-config-provider>
</template>

<script setup>
import DefaultLayout from '@/layouts/DefaultLayout.vue'
</script>

<style>
@font-face {
    font-family: 'HMFont';
    src: url('https://cdn.unifyz.5k.work/HarmonyOS_Sans_Medium.ttf');
    font-weight: normal;
    font-style: normal;
}
body {
    font-family: 'HMFont';
}
</style>