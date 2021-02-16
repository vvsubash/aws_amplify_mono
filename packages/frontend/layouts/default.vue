<template>
  <div>
    <div>
      <div v-if="authState !== 'signedin'">You are signed out.</div>
      <amplify-authenticator>
        <div v-if="authState === 'signedin' && user">
          <div>Hello, {{ user.username }}</div>
        </div>
        <amplify-sign-out></amplify-sign-out>
      </amplify-authenticator>
    </div>
    <Nuxt />
    <div class="container" v-if="authState === 'signedin'">
      <label for="taskNAme">Task Name</label>
      <input type="text" v-model="task" />
      <button @click="createTask">Click ME</button>
    </div>
  </div>
</template>
<script>
import { onAuthUIStateChange } from "@aws-amplify/ui-components";
import { DataStore } from "@aws-amplify/datastore";
import { PrivateNote, Task } from "../../newAmp/src/models";
import {
  createTask,
  createPrivateNote
} from "../../newAmp/src/graphql/mutations";
import { API } from "aws-amplify";

export default {
  created() {
    this.unsubscribeAuth = onAuthUIStateChange((authState, authData) => {
      this.authState = authState;
      this.user = authData;
    });
  },
  data() {
    return {
      user: undefined,
      authState: undefined,
      unsubscribeAuth: undefined,
      task: null
    };
  },
  methods: {
    async createTask() {
      const task = this.task;
      if (task === null) alert("task is Null");
      try {
        await API.graphql({
          query: createTask,
          variables: {
            input: {
              title: "jhbn",
              status: "jhbb",
              description: "hcgfc"
            }
          }
        });
      } catch (error) {
        console.log(error);
      }
    }
  },
  beforeDestroy() {
    this.unsubscribeAuth();
  }
};
</script>
<style>
html {
  font-family: "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI",
    Roboto, "Helvetica Neue", Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}
</style>
