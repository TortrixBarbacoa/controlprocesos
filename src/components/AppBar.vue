<template>
  <v-app-bar class="bg-dark" elevation="3">
    <template #prepend>
      <v-app-bar-nav-icon :flat="true">
        <v-icon>mdi-console</v-icon>
      </v-app-bar-nav-icon>
    </template>
    <v-app-bar-title class="font-weight-black">
      Control de Procesos
    </v-app-bar-title>
    <template #append>
      <v-icon size="40" class="mr-4">
        <v-img class="rounded-sm" src="../assets/logoUVG.jpg"></v-img>
      </v-icon>
    </template>
  </v-app-bar>
</template>
