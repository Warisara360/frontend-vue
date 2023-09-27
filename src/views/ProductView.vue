<template>
    <v-container>
      <v-row>
          <v-col cols= "12"> 
            <Product/>
  
          </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  import Product from '../components/Product.vue';
  
  export default {
      comments: {
        Product: () => import('../components/Product.vue')
      },
      components: { Product}
  }
  </script>
  
  <style>
  
  </style>