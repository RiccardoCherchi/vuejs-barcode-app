<template>
  <div v-if="post">
    <h1>Barcode created</h1>
    <p>barcode: {{post["barcode"]}}</p>
    <p>description: {{ post["description"] }}</p>
    <p>price: {{ post["price"] }}</p>
    <button v-on:click="location.reload()">view barcode list</button>
  </div>
  <div v-else-if="errors">
    <div v-for="(error, index) in errors" :key="index">
      <div v-for="(message, index) in error" :key="index">
        <p>{{message}}</p>
      </div>
    </div>
    <button v-on:click="close()">close</button>
  </div>
  <div v-else>
    <h1>Barcode list</h1>
    <button v-on:click="create()">add new barcode</button>
    <table class="table">
      <tr>
        <th scope="col">id</th>
        <th scope="col">Barcode</th>
        <th scope="col">Description</th>
        <th scope="col">Price</th>
      </tr>
      <tr>
        <tr v-for="(data, index) in barcodes.data" :key="index">
          <th>{{ index + 1 }}</th>
          <td><router-link :to="{ path: data['barcode'] }">{{ data["barcode"] }}</router-link></td>
          <td>{{ data["description"] }}</td>
          <td>{{ data["price"] }}</td>
        </tr>
    </table>
  </div>
</template>

<script>
  import axios from 'axios'

  const url = 'http://127.0.0.1:8000/';

export default {
  name: 'HelloWorld',
  data() {
    return {
      barcodes: null,
      post: null,
      errors: ''
    }
  },
  methods: {
    create: function () {
      // this.barcodes = false;
      axios
              .post(url, {
                "description": "test from vue",
                "price": 1000,
                "barcode": "1232533"
              })
              .then(
                      (response) => (this.post = response.data ),
                      (error) => (this.errors = error.response.data)
              )
    },
    close: function () {
      this.errors = null;
    }
  },
  mounted() {
    axios
            .get(url)
            .then(
                    (response) => ( this.barcodes = response ),
                    (error) => ( this.errors = error )
            )
    }
  }
</script>