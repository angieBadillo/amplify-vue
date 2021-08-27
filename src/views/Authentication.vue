<template>
  <b-container align="center">
    <b-card style="max-width: 40rem">
      <b-row class="mt-2">
        <b-col cols="6">
          <b-button
            block
            variant="primary"
            type="button"
            @click="googleSignIn()"
          >
            Google Signin
          </b-button>
        </b-col>
        <b-col>
          <b-button
            block
            variant="primary"
            type="button"
            @click="getCurrentAuthenticatedUser()"
          >
            Google authenticated cognitoUser
          </b-button>
        </b-col>
      </b-row>

      <b-row class="mt-2">
        <b-col cols="6">
          <b-button
            block
            variant="primary"
            type="button"
            @click="getCurrentUserInfo()"
          >
            Get current user info
          </b-button>
        </b-col>
        <b-col>
          <b-button
            block
            variant="primary"
            type="button"
            @click="getUserPoolUser()"
          >
            Get user pool user
          </b-button>
        </b-col>
      </b-row>

      <b-row class="mt-2">
        <b-col cols="6">
          <b-button
            block
            variant="primary"
            type="button"
            @click="getCredentials()"
          >
            Get current credentials
          </b-button>
        </b-col>
        <b-col>
          <b-button
            block
            variant="primary"
            type="button"
            @click="getEssentialCredentials()"
          >
            Essential credentials
          </b-button>
        </b-col>
      </b-row>

      <b-row class="mt-2">
        <b-col cols="6">
          <b-button
            block
            variant="primary"
            type="button"
            @click="getUserSession()"
          >
            User Session
          </b-button>
        </b-col>
        <b-col>
          <b-button
            block
            variant="primary"
            type="button"
            @click="getCurrentSession()"
          >
            Current Session
          </b-button>
        </b-col>
      </b-row>
    </b-card>
    <b-card
      style="max-width: 40rem"
      title="My Custom Login Form"
      v-if="show"
      class="mt-5"
    >
      <b-form>
        <b-form-input
          id="input-1"
          v-model="form.username"
          required
          placeholder="Enter username"
        >
        </b-form-input>
        <b-form-input
          id="input-2"
          v-model="form.password"
          required
          placeholder="Enter password"
          class="mt-3"
        >
        </b-form-input>
        <div class="mt-2">
          <b-button variant="primary" @click="login()"> Login </b-button>
          <b-button variant="danger" @click="onReset" class="ml-2">
            Reset
          </b-button>
        </div>
      </b-form>
    </b-card>
    <b-card v-else>
      <b-button variant="primary" @click="logout()">Logout</b-button>
    </b-card>
  </b-container>
</template>

<script>
export default {
  name: "App",
  async created() {},
  destroyed() {},
  components: {},
  data() {
    return {
      form: {
        username: "",
        password: "",
      },
      show: "true",
    };
  },
  methods: {
    async login() {
      console.log("In log in ...");
    },
    async logout() {
      console.log("logging out ...");
    },
    onReset() {
      console.log("onReset() ...");
    },
    async getCurrentUserInfo() {
      console.log("in getCurrentUserInfo ...");
    },
    getCurrentAuthenticatedUser() {
      console.log("in getCurrentAuthenticatedUser ...");
    },
    getUserPoolUser() {
      console.log("in getUserPoolUser ...");
    },
    getCredentials() {
      console.log("in getCredentials ...");
    },
    getEssentialCredentials() {
      console.log("in getEssentialCredentials ...");
    },
    getCurrentSession() {
      console.log("in getCurrentSession ...");
    },
    getUserSession() {
      console.log("in getUserSession ...");
    },
    googleSignIn() {
      console.log("in googleSignIn ...");
    },
  },
};
</script>
\\

let test = async function() {
    return axios.get("/api/result").then((data) => console.log(data));
}

console.log("A");
await test();
console.log("B");