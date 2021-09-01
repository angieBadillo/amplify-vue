<template>
  <b-container align="center">
    <b-card style="max-width: 40rem">
      <!-- <template>
        <amplify-authenticator> -->
          <!-- The rest of your app code -->
          <!-- <amplify-sign-out></amplify-sign-out>
        </amplify-authenticator> -->
      <!-- </template> -->
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
            @click="getCredential()"
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
    <b-card style="max-width: 40rem" title="Login" v-if="show" class="mt-5">
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
import { Auth } from "aws-amplify";
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
      console.log(
        "In log in ...username" +
          this.form.username +
          " password: " +
          this.form.password
      );
      try {
        const user = await Auth.signIn(this.form.username, this.form.password);
        console.log("Successful login" + JSON.stringify(user, null, 2));
        this.show = false;
        this.form.username = "";
        this.form.password = "";
      } catch (error) {
        console.log("Failure login: " + JSON.stringify(error, null, 2));
      }
    },
    async logout() {
      console.log("logging out ... ");
      try {
        await Auth.signOut();
        console.log("successfully logged out...");
        this.show = true;
      } catch (error) {
        console.log("error singing out: ", error);
      }
    },
    onReset(evt) {
      console.log("onReset() ...");
      evt.preventDefault();
      //Reset form values
      this.form.username = "";
      this.form.password = "";
      //Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show.true;
      });
    },
    async getCurrentUserInfo() {
      console.log("in getCurrentUserInfo ...");
      try {
        let currentUser = await Auth.currentUserInfo();
        console.log(
          "*** Current user information: " +
            JSON.stringify(currentUser, null, 2)
        );
      } catch (err) {
        console.log(
          "*** current user information error: " + JSON.stringify(err, null, 2)
        );
      }
    },
    getCurrentAuthenticatedUser() {
      console.log("in getCurrentAuthenticatedUser ...");
      Auth.currentAuthenticatedUser()
        .then((cognitoUser) => {
          console.log(
            "current user info: " + JSON.stringify(cognitoUser, null, 2)
          );
        })
        .catch((err) => {
          console.log(
            "current user info error: " + JSON.stringify(err, null, 2)
          );
        });
    },
    getUserPoolUser() {
      console.log("in getUserPoolUser ...");
      Auth.currentUserPoolUser()
        .then((authenticatedCognitoUser) => {
          console.log(
            "currentUserPoolUser info: " +
              JSON.stringify(authenticatedCognitoUser, null, 2)
          );
        })
        .catch((err) => {
          console.log("currentUserPoolUser: " + JSON.stringify(err, null, 2));
        });
    },
    getCredential() {
      console.log("in getCredentials ...");
      Auth.currentCredentials()
        .then((credentials) => {
          console.log(
            "currentUserCredentials: " + JSON.stringify(credentials, null, 2)
          );
        })
        .catch((err) => {
          console.log(
            "currentUserCredentials error!: " + JSON.stringify(err, null, 2)
          );
        });
    },
    getEssentialCredentials() {
      console.log("in getEssentialCredentials ...");
      Auth.currentCredentials().then((credentials) => {
        let essentialCredentials = Auth.essentialCredentials(credentials);
        console.log(
          "essentialCredentials: " +
            JSON.stringify(essentialCredentials, null, 2)
        );
      });
    },
    getCurrentSession() {
      console.log("in getCurrentSession ...");
      Auth.currentSession()
        .then((session) => {
          console.log("CurrentSession: " + JSON.stringify(session, null, 2));
        })
        .catch((err) => {
          console.log("CurrentSession error: " + JSON.stringify(err, null, 2));
        });
    },
    getUserSession() {
      Auth.currentUserPoolUser()
        .then((user) => {
          Auth.userSession(user)
            .then((session) => {
              console.log("UserSession: " + JSON.stringify(session, null, 2));
            })
            .catch((err) => {
              console.log("UserSession error: " + JSON.stringify(err, null, 2)
              );
            });
        })
        .catch((err) => {
          console.log(
            "currentUserPoolInfoError: " + JSON.stringify(err, null, 2)
          );
        });
    },
    googleSignIn() {
      //Auth.federatedSignIn({ provider: "Google" });
      Auth.federatedSignIn();
    },
  },
};
</script>
