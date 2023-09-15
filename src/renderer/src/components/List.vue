<template>
  <div class="List">
    <div clss="DivList">
      <div class="ListItem" v-for="i in d" :key="i">
        <h3>{{ i.title }}</h3>
        <h3>{{ i.des }}</h3>
      </div>
    </div>
  </div>
</template>
<script>
import songs from './songs.json'
export default {
  data() {
    return {
      i: 0,
      d: songs
    }
  }
}
</script>
<style scoped></style>
