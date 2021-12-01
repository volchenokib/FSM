<template>
  <el-form>
    <el-form-item>
      <el-input v-model="url" placeholder="enter the link to your website">
      </el-input>
    </el-form-item>
    <el-button type="primary" @click="sendUrl">Submit</el-button>
  </el-form>
</template>

<script>
/* eslint-disable */
export default {
  name: "App",
  data() {
    return {
      url: ''
    }
  },
  methods: {
    sendUrl() {
      console.log('url:', this.url)
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
