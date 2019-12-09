<template>
    <div v-if="succes">
        <h1>{{succes}} <router-link to="/">Go home</router-link></h1>
    </div>
    <div v-else-if="barcode">
        <h1>{{ barcode["description"] }}</h1>
        <h3>barcode: {{ barcode["barcode"] }}</h3>
        <h3>prezzo: {{ barcode["price"] }}</h3>
        <h3>aliquota {{ barcode["aliquota"] }}</h3>
        <button class="btn btn-danger mr-3" v-on:click="deleteBarcode()">delete</button>
        <button class="btn btn-primary" v-on:click="form = !form">update</button>
        <div v-if="form" class="mt-5">
            <form action=""  @submit.prevent="update" novalidate>
                <label for="description">descrizione:</label>
                <textarea name="description" id="description" cols="20" rows="3" class="form-control" v-model="descrizione"/>
                <div class="text-danger" v-if="errors">
                    <span v-if="errors.barcode">
                        {{errors.barcode[0]}}
                    </span>
                </div>
                <label for="barcode">Barcode</label>
                <input type="text" name="barcode" id="barcode" class="form-control" v-model="formBarcode">
                <div class="text-danger" v-if="errors">
                    <span v-if="errors.price">
                        {{errors.price[0]}}
                    </span>
                </div>
                <label for="price">prezzo: </label>
                <input type="number" name="price" id="price" class="form-control" v-model="prezzo">
                <div class="text-danger" v-if="errors">
                    <span v-if="errors.aliquota">
                        {{errors.aliquota[0]}}
                    </span>
                </div>
                <label for="aliquota">Aliquota IVA:</label>
                <input type="number" name="aliquota" id="aliquota" class="form-control" v-model="aliquota">
                <input type="submit" value="invia" class="btn btn-success mt-3">
            </form>
        </div>
    </div>
    <div v-else>
        <h1>Error {{ errors }} Not Found</h1>
    </div>
</template>

<script>
    import axios from 'axios'
    import 'bootstrap/dist/css/bootstrap.css'
    import 'jquery'
    import router from "@/router";
    // import router from "@/router";

    const url = 'http://93.144.11.209:8000/';

    export default {
        name: "Barcode",
        data() {
            return {
                barcode: null,
                errors: null,
                succes: null,
                form: false,
                descrizione: null,
                formBarcode: null,
                prezzo: null,
                aliquota: null,
            };
        },
        methods: {
          deleteBarcode: function () {
              axios
                  .delete(url + this.$route.params.barcode);
              this.succes = 'Barcode Delete succesfully'
          },
            update: function () {
                axios
                    .put(url + this.$route.params.barcode, {
                        "description": this.descrizione,
                        "price": this.prezzo,
                        "barcode": this.formBarcode,
                        "aliquota": this.aliquota
                    })
                    .then(
                        (response) => {this.barcode = response.data; this.errors = null; this.get(); this.clearFrom();
                                        router.push('/')},
                        (error) => (this.errors = error.response.data)
                    );
            },
            clearFrom: function () {
                this.descrizione = null;
                this.formBarcode = null;
                this.prezzo = null;
                this.aliquota = null;
                this.form = false;
            },
            get: function () {
                axios
                    .get(url + this.$route.params.barcode)
                    .then(
                        response => (this.barcode = response.data),
                        error => (this.errors = error.response.status)
                    );
            }
        },
        mounted() {
            axios
                .get(url + this.$route.params.barcode)
                .then(
                    response => {
                            this.barcode = response.data;
                            this.formBarcode = response.data.barcode;
                            this.descrizione = response.data.description;
                            this.prezzo = response.data.price;
                            this.aliquota = response.data.aliquota
                    },
                    error => (this.errors = error.response.status)
                );
        }
    }
</script>