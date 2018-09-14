<template>
  <div id="app">
    <textarea type="text" rows="5" v-model="jsondata"></textarea>
    <br/>
    <input type="text" v-model="apiurl" @keyup.enter="submit">
    <br/>
    <button @click="submitGet">get</button>
    <button @click="submitPost">post</button>
    <button @click="submitPut">put</button>
    <button @click="submitDelete">delete</button>
    <h1>Server Response:</h1>
    <p>{{ resp }}</p>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "app",
  data() {
    return {
      jsondata: "{key1: 'val1', key2: 0}",
      apiurl: "http://localhost:8081/api/",
      token: "",
      resp: "nothing"
    };
  },
  methods: {
    submitGet() {
      axios
        .get(this.apiurl, {
          headers: {
            Authorization: "JWT " + this.token
          }
        })
        .then(response => {
          this.resp = response.data;
        })
        .catch(error => {
          this.resp = JSON.stringify(error.response);
        });
    },
    submitPut() {
      var data = eval("(" + this.jsondata + ')');
      console.log(data);
      axios
        .put(this.apiurl, data, {
          headers: {
            "Content-type": "application/json",
            Authorization: "JWT " + this.token
          }
        })
        .then(response => {
          this.resp = response.data;
        })
        .catch(error => {
          this.resp = JSON.stringify(error.response);
        });
    },
    submitPost() {
      var data = eval("(" + this.jsondata + ')');
      console.log(data);
      axios
        .post(this.apiurl, data, {
          headers: {
            "Content-type": "application/json",
            Authorization: "JWT " + this.token
          }
        })
        .then(response => {
          this.resp = response.data;
        })
        .catch(error => {
          this.resp = JSON.stringify(error.response);
        });
    },
    submitDelete() {
      axios
        .delete(this.apiurl, {
          headers: {
            Authorization: "JWT " + this.token
          }
        })
        .then(response => {
          this.resp = response.data;
        })
        .catch(error => {
          this.resp = JSON.stringify(error.response);
        });
    },
  },
  created() {
    axios
      .post("http://127.0.0.1:8081/api/user/auth/obtain_token/", {
        username: "abcd",
        password: "abcd1234"
      })
      .then(response => {
        this.token = response.data.token;
      })
      .catch(error => {
        alert("error");
        console.log(error);
      });
  }
};
</script>

<style>
textarea,
input {
  width: 400px;
}
</style>
