<template>
  <span>
    <select>
      <option v-for="pageId of pageIds" v-bind:key="pageId">{{pageId}}</option>
    </select>
  </span>
</template>

<script>
export default {
  name: "SelectPageComponent",
  props: {
    pages: {
      type: Array
    }
  },
  computed: {
    pageIds() {
      return this.pages.map(page => page.id);
    }
  }
};
</script>
