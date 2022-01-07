<template>
  <el-form>
    <el-form-item>
      <el-input v-model="pageUrl" placeholder="enter the link to your website">
      </el-input>
    </el-form-item>
    <el-button type="primary" @click="sendUrl">Submit</el-button>
  </el-form>
  <h2>Heatmap</h2>
  <div
    class="heatmap__container"
    :style="{ backgroundImage: 'url(' + mediaLink + ')' }"
  ></div>
</template>

<script>
/* eslint-disable */
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      pageUrl: "",
      mediaLink:""
    };
  },
  methods: {
    async sendUrl() {
      console.log("pageUrl:", this.pageUrl);
      const response = await axios.post(
        "https://us-central1-heatmapp-get.cloudfunctions.net/screenshotProcessing",
        // "https://us-central1-heatmap-e4549.cloudfunctions.net/screenshotProcessing",
        // "http://localhost:5001/heatmap-e4549/us-central1/screenshotProcessing",
        { screenshotUrl: this.pageUrl }
      );
      console.log('response.data', response.data)
      this.mediaLink = response.data.mediaLink
    }
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.heatmap {
&__container {
  background-color: grey;
  height: 800px;
  width: 600px;
}
}
</style>
