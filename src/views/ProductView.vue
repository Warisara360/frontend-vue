<template>
    <v-container>
      <v-row>
          <v-col cols= "12"> 
            <Student/>
  
          </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  import Student from '../components/Product.vue';
  
  export default {
      comments: {
          Student: () => import('../components/Product.vue')
      },
      components: { Student }
  }
  </script>
  
  <style>
  
  </style>