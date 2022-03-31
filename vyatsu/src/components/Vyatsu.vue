<template>
  <iframe :srcdoc="results" id="frame" class="frame"></iframe>
</template>

<script lang="ts">
import axios from "axios";

const url = "http://localhost:8080/admission/";

export default {
  props: {
    results: [],
  },
  mounted() {
    axios
      .get(url)
      .then((response: any) => {
        console.log(response);
        this.results = response.data;
      })
      .catch((error: any) => console.log(error));
  },
};
</script>
<style scoped>
.frame {
  min-width: 480px;
  width: 100%;
  height: 600px;
}
</style>
