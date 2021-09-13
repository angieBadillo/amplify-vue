<template>
  <b-container align="center">
    <b-card style="max-width: 40rem;">
      <b-card
        align="right"
        sub-title="Add item to DynamoDb"
        class="text-center mt-3"
      >
        <b-card-text>
          Add a new item to database. Enter details below
        </b-card-text>
        <b-form-input
          v-model="dynamoDbItem.name"
          placeholder="Enter name"
          class="mt-2"
        ></b-form-input>
        <b-form-input
          v-model="dynamoDbItem.phone"
          placeholder="Enter phone"
          class="mt-2"
        ></b-form-input>
        <b-form-input
          v-model="dynamoDbItem.email"
          placeholder="Enter email"
          class="mt-2"
        ></b-form-input>
        <b-form-input
          v-model="dynamoDbItem.age"
          placeholder="Enter age"
          class="mt-2"
        ></b-form-input>
        <b-button variant="warning" @click="addItemToDynamoDb()" class="mt-2">
          Add item to DynamoDb database
        </b-button>
      </b-card>
      <b-card
        align="right"
        sub-title="Update item in DynamoDb"
        class="text-center mt-3"
      >
        <b-card-text>
          Update item in database. Enter item's identifying email address and
          the new values
        </b-card-text>

        <b-form-input
          v-model="dynamoDbItem.name"
          placeholder="Enter name"
          class="mt-2"
        ></b-form-input>
        <b-form-input
          v-model="dynamoDbItem.phone"
          placeholder="Enter phone"
          class="mt-2"
        ></b-form-input>
        <b-form-input
          v-model="dynamoDbItem.email"
          placeholder="Enter email"
          class="mt-2"
        ></b-form-input>
        <b-form-input
          v-model="dynamoDbItem.age"
          placeholder="Enter age"
          class="mt-2"
        ></b-form-input>
        <b-button variant="warning" @click="putData()" class="mt-2"
          >update item in DynamoDb database</b-button
        >
      </b-card>

      <b-card
        align="right"
        sub-title="List all DynamoDb items"
        class="text-center mt-3"
      >
        <b-card-text>
          List all the items in the database
        </b-card-text>
        <b-button variant="primary" @click="listData()"
          >List DynamoDb items</b-button
        >
      </b-card>

      <b-card
        align="right"
        sub-title="Get single DynamoDb item"
        class="text-center mt-3"
      >
        <b-card-text>
          Get single item out the database. Enter identifying email below
        </b-card-text>
        <b-form-input
          v-model="dynamoDbItem.email"
          placeholder="Enter email"
          class="mt-2"
        ></b-form-input>
        <b-button variant="primary" @click="getSingleItem()" class="mt-2"
          >Get single DynamoDb item</b-button
        >
      </b-card>

      <b-card
        align="right"
        sub-title="Delete DynamoDb item"
        class="text-center mt-3"
      >
        <b-card-text>
          Delete a single DynamoDb item. Enter identifying email address below
        </b-card-text>
        <b-form-input
          v-model="dynamoDbItem.email"
          placeholder="Enter email"
          class="mt-2"
        ></b-form-input>
        <b-button variant="danger" @click="deleteItem()" class="mt-2"
          >Delete DynamoDb item</b-button
        >
      </b-card>

      <b-button variant="danger" @click="getEvent()" class="mt-2"
        >Get event</b-button
      >
    </b-card>
  </b-container>
</template>

<script>
import { API, Auth } from "aws-amplify";

export default {
  name: "App",
  async created() {
    let userId = await this.getUserId();
    this.dynamoDbItem.userId = userId;
  },
  destroyed() {},
  components: {},
  data() {
    return {
      apiName: "restampvueapi",
      path: "/members",
      dynamoDbItem: {
        name: "",
        phone: null,
        email: "",
        age: null,
        userId: undefined,
      },
    };
  },
  methods: {
    deleteItem() {
      console.log(
        "in deleteItem(), delete item for: " + this.dynamoDbItem.email
      );

      const apiName = this.apiName;
      const path = this.path + "/object/" + this.dynamoDbItem.email;

      const myInit = {};

      API.del(apiName, path, myInit)
        .then((response) => {
          // Add your code here
          console.log(
            "delete item response: " + JSON.stringify(response, null, 2)
          );
        })
        .catch((error) => {
          console.log(
            "delete item response error: " +
              JSON.stringify(error.response, null, 2)
          );
        });
      this.dynamoDbItem.email = "";
    },
    async getUserId() {
      console.log("in getUserId()...");
      if (
        this.dynamoDbItem.userId != null ||
        typeof this.dynamoDbItem.userId != "undefined"
      ) {
        return this.dynamoDbItem.userId;
      } else {
        var currentUserInfo = await Auth.currentUserInfo();
        if (currentUserInfo) {
          this.dynamoDbItem.userId = currentUserInfo.id;
          return this.dynamoDbItem.userId;
        } else {
          return undefined;
        }
      }
    },
    async getEvent() {
      console.log("in getEvent()...");

      let userId = await this.getUserId();
      const apiName = this.apiName;
      let path = this.path;

      if (userId) {
        path = path + "/event?userId=" + userId;
      } else {
        path = path + "/event?userId=" + "UNAUTH";
      }

      try {
        const items = await API.get(apiName, path, {});
        console.log("result: " + JSON.stringify(items, null, 2));
      } catch (err) {
        console.log("result: " + JSON.stringify(err, null, 2));
      }
      this.dynamoDbItem.email = "";
    },
    async getSingleItem() {
      const apiName = this.apiName;
      const path = this.path + "/object/" + this.dynamoDbItem.email;

      try {
        const items = await API.get(apiName, path, {});
        console.log("result: " + JSON.stringify(items, null, 2));
      } catch (err) {
        console.log("result: " + JSON.stringify(err, null, 2));
      }
      this.dynamoDbItem.email = "";
    },
    async listData() {
      console.log("in listData()....");

      const apiName = this.apiName;
      let path = this.path + "/list";

      const myInit = {
        headers: {},
      };

      API.get(apiName, path, myInit)
        .then((response) => {
          // Add your code here
          console.log(
            "listData response: " + JSON.stringify(response, null, 2)
          );
        })
        .catch((error) => {
          console.log(
            "listData response error: " +
              JSON.stringify(error.response, null, 2)
          );
        });
    },
    async putData() {
      console.log("updating item...");
      const apiName = this.apiName;
      const path = this.path;

      const myInit = {
        body: {
          age: this.dynamoDbItem.age,
          email: this.dynamoDbItem.email,
          name: this.dynamoDbItem.name,
          phone: this.dynamoDbItem.phone,
        },
      };

      try {
        let result = await API.put(apiName, path, myInit);
        console.log("update result: " + JSON.stringify(result, null, 2));
      } catch (err) {
        console.log("update error: " + JSON.stringify(err, null, 2));
      }
      this.dynamoDbItem.age = null;
      this.dynamoDbItem.email = "";
    },
    async addItemToDynamoDb() {
      console.log("in addItemDynamoDb....");

      let myBody = {
        phone: this.dynamoDbItem.phone,
        name: this.dynamoDbItem.name,
        email: this.dynamoDbItem.email,
        age: this.dynamoDbItem.age,
      };

      let path = this.path;

      try {
        var postResponse = await API.post(this.apiName, path, { body: myBody });
        console.log("put response: " + JSON.stringify(postResponse, null, 2));
      } catch (err) {
        console.log("put error: " + JSON.stringify(err, null, 2));
      }
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
</style>
