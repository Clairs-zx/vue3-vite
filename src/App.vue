<template>
  <el-config-provider :local="local"></el-config-provider>
</template>

<script setup lang="ts">
// 无法找到模块报错解决：vite-env.d.ts中添加：declare module "element-plus/dist/locale/zh-cn.mjs";
import zhCn from 'element-plus/dist/locale/zh-cn.mjs'

const local = zhCn
</script>

<style scoped></style>
