<template>
   <div style="height: 100%">
      <v-data-table class="pa-2" dense :items="rows" :headers="headers" :items-per-page="-1" hide-default-footer :elevation="4">
         <template v-slot:top>
            <v-toolbar class="">
               <v-btn @click="addItem" class="d-none d-sm-flex mr-2"><v-icon left>mdi-plus-circle</v-icon>Agregar</v-btn>
               <v-btn @click="addItem" class="d-flex d-sm-none mr-2" icon small><v-icon small>mdi-plus-circle</v-icon></v-btn>
               <v-btn @click="clear" class="d-none d-sm-flex mr-2"><v-icon left>mdi-eraser</v-icon>Limpiar</v-btn>
               <v-btn @click="clear" class="d-flex d-sm-none mr-2" icon small><v-icon small>mdi-eraser</v-icon></v-btn>
               <v-btn @click="reset" class="d-none d-sm-flex mr-2"><v-icon left>mdi-autorenew</v-icon>Reiniciar</v-btn>
               <v-btn @click="reset" class="d-flex d-sm-none mr-2" icon small><v-icon small>mdi-autorenew</v-icon></v-btn>
            </v-toolbar>
         </template>
         <template v-slot:[`item.name`]="{ item }">
            <v-text-field @blur="save" @input="save" v-model="item.name">{{ item.name }}</v-text-field>
         </template>
         <template v-slot:[`item.cost`]="{ item }">
            <div class="d-flex align-center">
               <v-text-field
                  @blur="save"
                  @input="save"
                  prepend-icon="mdi-currency-usd"
                  prefix="/pz"
                  class="mx-2"
                  type="number"
                  reverse
                  v-model="item.cost"
                  >{{ item.cost }}</v-text-field
               >
            </div>
         </template>
         <template v-slot:[`item.init`]="{ item }">
            <div class="d-flex align-center">
               <v-text-field
                  class="mx-2"
                  reverse
                  prefix="/pz"
                  type="number"
                  @focus="
                     () => {
                        item.init = '';
                     }
                  "
                  @blur="
                     () => {
                        item.init = item.init == '' ? 0 : item.init;
                     }
                  "
                  step="1"
                  v-model="item.init"
                  >{{ item.init }}</v-text-field
               >
            </div>
         </template>
         <template v-slot:[`item.final`]="{ item }">
            <div class="d-flex align-center">
               <v-text-field
                  class="mx-2"
                  reverse
                  prefix="/pz"
                  type="number"
                  @focus="
                     () => {
                        item.final = '';
                     }
                  "
                  @blur="
                     () => {
                        item.final = item.final == '' ? 0 : item.final;
                     }
                  "
                  step="1"
                  v-model="item.final"
                  >{{ item.final }}</v-text-field
               >
            </div>
         </template>
         <template v-slot:[`item.total`]="{ item }">
            <div class="d-flex">
               <p class="mb-0 mx-2"><v-icon x-small>mdi-currency-usd</v-icon>{{ calcRow(item) }}</p>
               <p class="mb-0 mx-2"><v-icon x-small>mdi-logout-variant</v-icon>{{ calcRowDev(item) }}</p>
            </div>
         </template>
         <template v-slot:[`item.actions`]="{ item }">
            <v-btn icon x-small @click="deleteItem(item)"><v-icon>mdi-close</v-icon></v-btn>
         </template>
      </v-data-table>
      <div class="d-flex align-center justify-center flex-md-row ma-3" style="width: 100%">
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
         { text: "", value: "actions", sortable: false },
      ],
      rows: [],
   }),
   created: function () {
      try {
         if (localStorage.getItem("rows")) {
            this.rows = JSON.parse(localStorage.getItem("rows"));
         } else {
            throw "not init rows";
         }
      } catch (error) {
         this.rows = [
            {
               name: "Peque",
               cost: 1.3,
               init: 0,
               final: 0,
            },
            {
               name: "Bonice",
               cost: 2,
               init: 0,
               final: 0,
            },
            {
               name: "Doble",
               cost: 4,
               init: 0,
               final: 0,
            },
            {
               name: "Bonicessote",
               cost: 4,
               init: 0,
               final: 0,
            },
            {
               name: "Mega",
               cost: 6.5,
               init: 0,
               final: 0,
            },
            {
               name: "Doblessote",
               cost: 6.5,
               init: 0,
               final: 0,
            },
         ];
      }
   },
   methods: {
      save() {
         let saveRows = [];
         this.rows.forEach((element) => {
            saveRows.push({
               name: element.name,
               cost: element.cost,
               init: 0,
               final: 0,
            });
         });
         localStorage.setItem("rows", JSON.stringify(saveRows));
      },
      clear() {
         let clearRows = [];
         this.rows.forEach((element) => {
            clearRows.push({
               name: element.name,
               cost: element.cost,
               init: 0,
               final: 0,
            });
         });
         this.rows = clearRows;
      },
      calcRow(item) {
         return ((item.init - item.final) * item.cost).toFixed(2);
      },
      calcRowDev(item) {
         return (item.init - item.final).toFixed(0);
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
      addItem() {
         if (this.rows.length <= 30) {
            this.rows.push({
               name: "",
               cost: 0,
               init: 0,
               final: 0,
            });
            this.save();
         }
      },
      deleteItem(item) {
         this.rows.splice(this.rows.indexOf(item), 1);
         this.save();
      },
      reset() {
         this.rows = [
            {
               name: "Peque",
               cost: 1.3,
               init: 0,
               final: 0,
            },
            {
               name: "Bonice",
               cost: 2,
               init: 0,
               final: 0,
            },
            {
               name: "Doble",
               cost: 4,
               init: 0,
               final: 0,
            },
            {
               name: "Bonicessote",
               cost: 4,
               init: 0,
               final: 0,
            },
            {
               name: "Mega",
               cost: 6.5,
               init: 0,
               final: 0,
            },
            {
               name: "Doblessote",
               cost: 6.5,
               init: 0,
               final: 0,
            },
         ];
         this.save();
      },
   },
};
</script>
