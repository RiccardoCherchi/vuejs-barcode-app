<template>
    <div v-if="succes">
        <h1>{{succes}} <router-link to="/">Go home</router-link></h1>
    </div>
    <div v-else-if="barcode">
        <h1>barcode: {{ barcode["barcode"] }}</h1>
        <h2>descrizione: {{ barcode["description"] }}</h2>
        <h3>prezzo: {{ barcode["price"] }}</h3>
        <button v-on:click="deleteBarcode()">delete</button>
        <button>update</button>
    </div>
    <div v-else>
        <h1>{{ errors }}</h1>
    </div>
</template>

<script>
    import axios from 'axios'
    // import router from "@/router";

    const url = 'http://127.0.0.1:8000/';

    export default {
        name: "Barcode",
        data() {
            return {
                barcode: false,
                errors: null,
                succes: null,
            };
        },
        methods: {
          deleteBarcode: function () {
              axios
                  .delete(url + this.$route.params.barcode);
              this.succes = 'Barcode Delete succesfully'
          }
        },
        mounted() {
            axios
                .get(url + this.$route.params.barcode)
                .then(
                    response => (this.barcode = response.data),
                    error => (this.errors = error)
                )
        }
    }
</script>