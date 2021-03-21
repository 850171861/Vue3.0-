<template>
  <div>
    {{result}}
  </div>
</template>

<script lang="ts">
import axios from "axios";
import { defineComponent } from "vue";
export default defineComponent({
  async setup() {
    const result = await axios.get("http://www.wudongming.com:3001/api/articlelist?status=1&page=1&limit=1");

    return {
      result: result.data,
    };
  },
});
</script>

<style>
</style>