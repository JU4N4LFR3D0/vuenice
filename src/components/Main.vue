<template>
   <div style="height: 100%">
      <v-data-table :items="rows" :headers="headers" :items-per-page="-1" dense hide-default-footer :elevation="4">
         <template v-slot:[`item.name`]="{ item }">
            <v-text-field v-model="item.name">{{ item.name }}</v-text-field>
         </template>
         <template v-slot:[`item.cost`]="{ item }">
            <div class="d-flex align-center">
               <v-text-field prepend-icon="mdi-currency-usd" prefix="/pz" class="mx-2" type="number" reverse v-model="item.cost">{{
                  item.cost
               }}</v-text-field>
            </div>
         </template>
         <template v-slot:[`item.init`]="{ item }">
            <div class="d-flex align-center">
               <v-text-field class="mx-2" reverse prefix="/pz" type="number" step="1" v-model="item.init">{{ item.init }}</v-text-field>
            </div>
         </template>
         <template v-slot:[`item.final`]="{ item }">
            <div class="d-flex align-center">
               <v-text-field class="mx-2" reverse prefix="/pz" type="number" step="1" v-model="item.final">{{ item.final }}</v-text-field>
            </div>
         </template>
         <template v-slot:[`item.total`]="{ item }">
            <p style="margin: 0"><v-icon x-small>mdi-currency-usd</v-icon>{{ calcRow(item) }}</p>
         </template>
         <template v-slot:footer> </template>
      </v-data-table>
      <div class="d-flex align-center justify-center flex-md-row" style="width: 100%">
         <v-chip class="ma-2" large color="error">
            <div class="d-flex d-sm-none">
               <v-icon>mdi-dolly</v-icon>
            </div>
            <div class="d-none d-sm-flex">Piezas cargadas:</div>
            {{ calcLoaded() }}
         </v-chip>
         <v-chip class="ma-2" large color="warning">
            <div class="d-flex d-sm-none">
               <v-icon>mdi-hand-coin</v-icon>
            </div>
            <div class="d-none d-sm-flex">Piezas vendidas:</div>
            {{ calcSold() }}
         </v-chip>
         <v-chip class="ma-2" large color="success">
            <div class="d-flex d-sm-none">
               <v-icon>mdi-cash-register</v-icon>
            </div>
            <div class="d-none d-sm-flex">Total:</div>
            <v-icon x-small>mdi-currency-usd</v-icon>{{ calcTotal() }}
         </v-chip>
      </div>
   </div>
</template>

<script>
export default {
   name: "Main",
   data: () => ({
      headers: [
         { text: "Nombre", value: "name", sortable: false },
         { text: "Costo/Pieza", value: "cost", sortable: false },
         { text: "Piezas", value: "init", sortable: false },
         { text: "Devolución", value: "final", sortable: false },
         { text: "Total", value: "total", sortable: false },
      ],
      rows: [
         {
            name: "Peque",
            cost: 1.3,
            init: 0,
            final: 0,
            total: 0,
         },
         {
            name: "Bonice",
            cost: 2,
            init: 0,
            final: 0,
            total: 0,
         },
         {
            name: "Doble",
            cost: 4,
            init: 0,
            final: 0,
            total: 0,
         },
         {
            name: "Bonicessote",
            cost: 4,
            init: 0,
            final: 0,
            total: 0,
         },
      ],
   }),
   methods: {
      calcRow(item) {
         return ((item.init - item.final) * item.cost).toFixed(2);
      },
      calcLoaded() {
         let loaded = 0;
         this.rows.forEach((element) => {
            loaded += parseFloat(element.init);
         });
         return loaded.toFixed(0);
      },
      calcSold() {
         let sold = 0;
         this.rows.forEach((element) => {
            sold += parseFloat(element.init) - parseFloat(element.final);
         });
         return sold.toFixed(0);
      },
      calcTotal() {
         let total = 0;
         this.rows.forEach((element) => {
            total += (parseFloat(element.init) - parseFloat(element.final)) * parseFloat(element.cost);
         });
         return total.toFixed(2);
      },
   },
};
</script>
