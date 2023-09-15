<script setup>
import List from './components/List.vue'
import Footers from './components/Footer.vue'
import songs from './components/songs.json'
</script>

<template>
  <img src="./assets/icons.svg" />
  <List />
  <Footers />
</template>

<style lang="less">
@import './assets/css/styles.less';
</style>
