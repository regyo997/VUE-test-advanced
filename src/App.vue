<template>
  <div class="app">
    <PageComponent/>
  </div>
</template>

<script>
import PageComponent from "./components/PageComponent.vue";

export default {
  name: "App",
  components: {
    PageComponent
  }
};
</script>