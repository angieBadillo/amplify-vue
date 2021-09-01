<template>
  <b-container>
    <div>
      <b-row align-h="center">
        <b-card>
          <template>
            <amplify-authenticator>
              <!-- The rest of your app code -->
              <amplify-sign-out></amplify-sign-out>
            </amplify-authenticator>
          </template>
        </b-card>
      </b-row>
    </div>
  </b-container>
</template>

<script>
export default {
  name: "Home",
  components: {},
  created() {
      onAuthUIStateChange((authState, authData) => {
          this.authState = authState;
          this.user = authData;
      })
  }
};
</script>
