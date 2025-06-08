<template>
  <v-container fluid class="fill-height align-center justify-center">
    <v-row class="px-5">
      <v-col v-for="(item, index) in items" :key="index" cols="12" sm="4">
        <v-card
          class="ma-2 pa-10 bg-white d-flex flex-column align-center"
          rounded="lg"
          style="cursor: pointer"
          elevation="8"
          variant="outlined"
          color="indigo-darken-1"
        >
          <v-icon class="mb-2" size="40">{{ item.icon }}</v-icon>
          <h3 class="text-h6 mb-1">{{ item.title }}</h3>
          <p class="text-subtitle-2 text-center">{{ item.description }}</p>
          <v-btn class="mt-10 bg-indigo-darken-1" @click="openModal(item)">
            Ejecutar Script
          </v-btn>
        </v-card>
      </v-col>
    </v-row>
  </v-container>

  <v-dialog
    v-model="modalVisible"
    max-width="600"
    class="rounded-md"
    persistent
  >
    <v-card class="pa-8 bg-white">
      <v-card-title class="font-weight-black text-center text-h6 mb-10">
        Ejecutar Script: {{ selectedItem.title }}
      </v-card-title>
      <v-form v-model="formRef">
        <v-row class="d-flex flex-column align-center">
          <v-col cols="12" sm="10">
            <v-number-input
              label="Frecuencia de ejecución (segundos)"
              variant="solo"
              control-variant="hidden"
              prepend-icon="mdi-timer-cog"
              bg-color="white"
              hint="Ingresa cada cuánto se ejecutará el script"
              required
              :rules="[(v) => !!v || 'Este campo es obligatorio']"
              v-model="paramsScript.intervalo"
            ></v-number-input>
          </v-col>
          <v-col cols="12" sm="10">
            <v-number-input
              label="Cantidad de Iteraciones"
              variant="solo"
              control-variant="hidden"
              prepend-icon="mdi-repeat-variant"
              bg-color="white"
              hint="Ingresa cuántas veces quieres que se ejecute el script"
              required
              :rules="[(v) => !!v || 'Este campo es obligatorio']"
              v-model="paramsScript.iteraciones"
            ></v-number-input>
          </v-col>
        </v-row>
      </v-form>
      <v-row class="d-flex align-center justify-center mt-10">
        <v-col cols="12" sm="3" class="d-flex align-center justify-center">
          <v-btn
            @click="executeScript()"
            color="success"
            size="small"
            class="rounded-sm"
          >
            Ejecutar
          </v-btn>
        </v-col>
        <v-col cols="12" sm="3" class="d-flex align-center justify-center">
          <v-btn
            @click="closeModal()"
            color="error"
            size="small"
            class="rounded-sm"
          >
            Cancelar
          </v-btn>
        </v-col>
      </v-row>
    </v-card>
  </v-dialog>
</template>

<script setup lang="ts">
import api from "@/plugins/axios";
import axios from "axios";
import Color from "vuetify/directives/color";

const modalVisible = ref(false);
const formRef = ref();
const headers = ref([
  { text: "Título", value: "title" },
  { text: "Icono", value: "icon" },
  { text: "Descripción", value: "description" },
]);
const items = [
  {
    title: "Procesos Activos",
    icon: "mdi-console",
    description: "Ejecutar script para ver procesos activos del sistema",
    color: "red",
  },
  {
    title: "Memoria",
    icon: "mdi-memory",
    description: "Ver uso de memoria del sistema",
    color: "green",
  },
  {
    title: "Almacenamiento",
    icon: "mdi-nas",
    description: "Ver uso de almacenamiento del sistema",
    color: "blue",
  },
];
const selectedItem = ref({
  title: "",
  icon: "",
  description: "",
});
const paramsScript = ref({
  intervalo: null,
  iteraciones: null,
});
const openModal = (item: (typeof items)[0]) => {
  selectedItem.value = item;
  console.log(selectedItem);
  modalVisible.value = true;
};
const closeModal = () => {
  paramsScript.value = {
    intervalo: null,
    iteraciones: null,
  };
  modalVisible.value = false; // Cierra el modal
};
const executeScript = async () => {
  try {
    const iteraciones = paramsScript.value.iteraciones;
    const pausa = paramsScript.value.intervalo;

    const response = await api.get("/verProcesos", {
      params: {
        iteraciones,
        pausa,
      },
    });

    console.log("Respuesta del backend:", response);
  } catch (error) {
    console.error("Error al ejecutar el script:", error);
  }
};
</script>
