<template>
  <h2>Finite-state machine</h2>

  <el-button @click="send('TOGGLE')">
    {{
      state.value === "inactive"
        ? "Click to activate"
        : "Active! Click to deactivate"
    }}
  </el-button>

  <el-form>
    <el-form-item>
      <el-input v-model="pageUrl" placeholder="enter the link to your website">
      </el-input>
    </el-form-item>
    <el-button type="primary" @click="sendUrl">Submit</el-button>
  </el-form>
</template>

<script>
/* eslint-disable */
import { ref } from 'vue'
import axios from "axios";
import { useMachine } from '@xstate/vue';
import { createMachine } from 'xstate';
import { getSimplePaths } from '@xstate/graph';
import { interpret } from 'xstate';


const toggleMachine = createMachine({
  id: 'toggle',
  initial: 'inactive',
  states: {
    inactive: {
      on: { TOGGLE: 'active' }
    },
    active: {
      on: { TOGGLE: 'inactive' }
    }
  }
});

const service = interpret(toggleMachine, { devTools: false });
const paths = getSimplePaths(toggleMachine);

export default {
  name: "App",
  setup() {
     const { state, send } = useMachine(toggleMachine, { devTools: true });
     
     const pageUrl = ref('');
     const mediaLink = ref('');

     const  sendUrl = async ()=> {
      console.log("pageUrl:", pageUrl.value);
      const response = await axios.post(
        "https://us-central1-heatmapp-get.cloudfunctions.net/screenshotProcessing",
        // "https://us-central1-heatmap-e4549.cloudfunctions.net/screenshotProcessing",
        // "http://localhost:5001/heatmap-e4549/us-central1/screenshotProcessing",
        { screenshotUrl: pageUrl.value }
      );
      console.log('response.data', response.data)
      mediaLink.value = response.data.mediaLink
    }

    return {
      pageUrl,
      mediaLink,
      sendUrl,
      state,
      send
    };
  }

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
