<template>
  <v-container>
    <v-row>
        <v-col cols= "12"> 
          <Table/>

        </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Table from '../components/Table.vue';

export default {
    comments: {
        Table: () => import('../components/Table.vue')
    },
    components: { Table }
}
</script>

<style>

</style>