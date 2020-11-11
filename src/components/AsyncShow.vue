<template>
  <h1 v-if="result">supense!!!</h1>
  <!-- <img :src="result && result.message" alt=""> -->
</template>


<script lang="ts">
import axios from "axios";
import { defineComponent } from "vue";

export default defineComponent({
  async setup() {
    // return new Promise(resovle => {
    //   setTimeout(() => {
    //     return resovle({
    //       result: "异步组件supense!"
    //     });
    //   }, 2000);
    // });
    const rawData = await axios.get("https://dog.ceo/api/breeds/image/random");
    console.log("rawData", rawData.data);
    return {
      result: rawData.data
    };
  }
});
</script>