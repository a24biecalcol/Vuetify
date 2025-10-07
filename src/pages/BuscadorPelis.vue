<template>
    <v-layout class="rounded rounded-md border">
        <v-app-bar title="Cercador de Pel·licules"></v-app-bar>
        <v-text-field v-model="textCerca" />
        <v-btn @click="enviarPeticio">Cercar </v-btn>
    </v-layout>
    <v-text-field v-model="textCerca" />
    <v-btn @click="enviarPeticio">Cercar </v-btn>
    <v-row>
        <v-col cols="3" v-for="actual in pelicules" :key="actual.imdbID">
            <v-card class="mx-auto" max-width="344">
                <v-img :src="actual.Poster" cover></v-img>
                <v-card-title> {{ actual.Title }} </v-card-title>
                <v-card-subtitle> {{ actual.Year }} </v-card-subtitle>
                <v-card-actions>
                    <v-btn @click="demanarMesInfo(actual.imdbID)" color="orange-lighten-2">Més informació</v-btn>
                    <v-spacer></v-spacer>
                </v-card-actions>
            </v-card>
        </v-col>
    </v-row>
    <v-dialog v-model="mostrarModal" max-width="600">
        <v-card>
            <v-card-title class="headline">
                {{ infoDetallada.Title }}
            </v-card-title>

            <v-card-text>
                <v-img :src="infoDetallada.Poster" height="300px" class="mb-4" cover></v-img>
                <p><strong>Any:</strong> {{ infoDetallada.Year }}</p>
                <p><strong>Director:</strong> {{ infoDetallada.Director }}</p>
                <p><strong>Actors:</strong> {{ infoDetallada.Actors }}</p>
                <p><strong>Gènere:</strong> {{ infoDetallada.Genre }}</p>
                <p><strong>Argument:</strong> {{ infoDetallada.Plot }}</p>
            </v-card-text>

            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" text @click="mostrarModal = false">Tancar</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>


<script setup>

import { ref } from "vue"

const textCerca = ref("")
const pelicules = ref([])
const infoDetallada = ref({})
const mostrarModal = ref(false)

const API_KEY = "e59bb7b2"
const BASE_URL = "http://www.omdbapi.com/"

async function enviarPeticio() {
    const response = await fetch(`${BASE_URL}?s=${textCerca.value}&apikey=${API_KEY}`)
    const data = await response.json()
    pelicules.value = data.Search || []
}

async function demanarMesInfo(id) {
    const response = await fetch(`${BASE_URL}?i=${id}&apikey=${API_KEY}`)
    const data = await response.json()
    infoDetallada.value = data
    mostrarModal.value = true
}
</script>