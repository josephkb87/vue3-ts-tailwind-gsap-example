<template>
<div>
  <home/>
</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Home from '@/components/Home/index.vue'
export default defineComponent({
  name: "HomeViews",
  components: {
    Home
  },
});
</script>


<style>

</style>