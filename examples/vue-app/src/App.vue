<template>
  <div id="app">
    <div>
      <span :style="{ marginRight: '20px' }">verifier:</span>
      <select v-model="selectedVerifier">
        <option value="google">Google</option>
        <option value="facebook">Facebook</option>
        <option value="twitch">Twitch</option>
        <option value="discord">Discord</option>
      </select>
    </div>
    <div :style="{ marginTop: '20px' }">
      <button @click="login">Login with Torus</button>
    </div>
    <div id="console">
      <p></p>
    </div>
  </div>
</template>

<script>
import TorusSdk from "@toruslabs/torus-direct-web-sdk";

export default {
  name: "App",
  data() {
    return {
      selectedVerifier: "google",
    };
  },
  methods: {
    async login() {
      try {
        const torusdirectsdk = new TorusSdk({
          baseUrl: "http://localhost:3000/serviceworker",
          GOOGLE_CLIENT_ID: "876733105116-i0hj3s53qiio5k95prpfmj0hp0gmgtor.apps.googleusercontent.com",
          FACEBOOK_CLIENT_ID: "2554219104599979",
          enableLogging: true,
        });
        await torusdirectsdk.init();
        const loginDetails = await torusdirectsdk.triggerLogin(this.selectedVerifier, this.selectedVerifier);
        this.console(loginDetails);
      } catch (error) {
        console.error(error);
      }
    },
    console(text) {
      document.querySelector("#console>p").innerHTML = typeof text === "object" ? JSON.stringify(text) : text;
    },
  },
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
#console {
  border: 1px solid black;
  height: 80px;
  padding: 2px;
  bottom: 10px;
  position: absolute;
  text-align: left;
  width: calc(100% - 20px);
  border-radius: 5px;
}
#console::before {
  content: "Console :";
  position: absolute;
  top: -20px;
  font-size: 12px;
}
#console > p {
  margin: 0.5em;
  word-wrap: break-word;
}
</style>
