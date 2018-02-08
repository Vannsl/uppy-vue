<template>
  <div class="grid">
    <div v-for="file in files" :key="file.id">
      <img :src="file.preview" />
    </div>
  </div>
</template>

<style>
  .grid {
    display: flex;
    flex-wrap: wrap;
  }
</style>


<script>
export default {
  name: 'photobox-component',
  props: ['files'],

  data() {
    return {};
  },

};
</script>
