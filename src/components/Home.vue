<template>
    <!--  <div v-if="post">-->
    <!--    <h1>Barcode created</h1>-->
    <!--    <p>barcode: {{post["barcode"]}}</p>-->
    <!--    <p>description: {{ post["description"] }}</p>-->
    <!--    <p>price: {{ post["price"] }}</p>-->
    <!--    <button v-on:click="close_post()">view barcode list</button>-->
    <!--  </div>-->
    <div>
        <!--    <div v-if="post" class="alert alert-success">{{ post.description }} was added</div>-->
        <div v-if="post" class="alert alert-success alert-dismissible fade show" role="alert">
            {{ post.description }} was added
            <button type="button" class="close" data-dismiss="alert" aria-label="Close" style="outline: none">
                <span aria-hidden="true">&times;</span>
            </button>
        </div>
        <h1>Barcode list</h1>
        <div v-if="errors">
            <!--      <div v-for="(error, index) in errors" :key="index">-->
            <!--        <div v-for="(message, index) in error" :key="index">-->
            <!--          <p>{{message}}</p>-->
            <!--        </div>-->
            <!--      </div>-->
<!--            {{errors}}-->
        </div>
        <div v-if="showForm">
            <form action="" @submit.prevent="onSubmit" novalidate>
                <label for="description">descrizione:</label>
                <textarea name="description" id="description" cols="20" rows="3" class="form-control"
                          v-model="descrizione"/>
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
        <button class="my-3 btn btn-primary" v-on:click="showForm = !showForm">add new barcode</button>
        <table class="table">
            <tr>
                <th scope="col">id</th>
                <th scope="col">Barcode</th>
                <th scope="col">Description</th>
                <th scope="col">Price</th>
                <th scope="col">Aliquota</th>
            </tr>
            <tr>
            <tr v-for="(data, index) in barcodes.data" :key="index">
                <th>{{ index + 1 }}</th>
                <td>
                    <router-link :to="{ path: data['barcode'] }">{{ data["barcode"] }}</router-link>
                </td>
                <td>{{ data["description"] }}</td>
                <td>{{ data["price"] }}</td>
                <td>{{data["aliquota"]}}%</td>
            </tr>
        </table>
    </div>
</template>

<script>
    import 'bootstrap/dist/css/bootstrap.css'
    import 'bootstrap'
    import 'jquery'
    import axios from 'axios'

    const url = 'http://93.144.11.209:8000/';

    export default {
        name: 'HelloWorld',
        data() {
            return {
                descrizione: null,
                formBarcode: null,
                prezzo: null,
                aliquota: null,
                barcodes: null,
                post: null,
                errors: null,
                showForm: false,
            }
        },
        methods: {
            onSubmit: function () {
                axios
                    .post(url, {
                        "description": this.descrizione,
                        "price": this.prezzo,
                        "barcode": this.formBarcode,
                        "aliquota": this.aliquota
                    })
                    .then(
                        (response) => {
                            this.post = response.data;
                            this.errors = null;
                            this.get();
                            this.clearFrom()
                        },
                        (error) => (this.errors = error.response.data)
                    );
            },
            get: function () {
                axios
                    .get(url)
                    .then(
                        (response) => (this.barcodes = response),
                        (error) => (this.errors = error)
                    )
            },
            clearFrom: function () {
                this.descrizione = null;
                this.formBarcode = null;
                this.prezzo = null;
                this.aliquota = null;
                this.showForm = false;
            }
        },
        mounted() {
            axios
                .get(url)
                .then(
                    (response) => (this.barcodes = response),
                    (error) => (this.errors = error)
                )
        }
    }
</script>

<style>
    body {
        margin: 15px;
    }
</style>