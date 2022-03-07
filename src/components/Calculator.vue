<template>
   <div>
      <v-dialog v-model="dialog" transition="dialog-bottom-transition" max-width="400">
         <v-card>
            <v-toolbar elevation="1">
               Calculadora
               <v-spacer></v-spacer>
               <v-btn class="mr-1" @click="calc = ''" icon small>
                  <v-icon small> mdi-eraser </v-icon>
               </v-btn>
               <v-btn class="mr-1" @click="close" icon small>
                  <v-icon small> mdi-close </v-icon>
               </v-btn>
            </v-toolbar>
            <v-card-text>
               <v-text-field :value="calc" outlined readonly class="mt-3" append-icon="mdi-backspace-outline" @click:append="erase"></v-text-field>
               <v-row>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('7')" x-large icon elevation="2">
                        <v-icon> mdi-numeric-7 </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('8')" x-large icon elevation="2">
                        <v-icon> mdi-numeric-8 </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('9')" x-large icon elevation="2">
                        <v-icon> mdi-numeric-9 </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('/')" x-large icon elevation="2">
                        <v-icon> mdi-division </v-icon>
                     </v-btn>
                  </v-col>
               </v-row>
               <v-row>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('4')" x-large icon elevation="2">
                        <v-icon> mdi-numeric-4 </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('5')" x-large icon elevation="2">
                        <v-icon> mdi-numeric-5 </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('6')" x-large icon elevation="2">
                        <v-icon> mdi-numeric-6 </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('*')" x-large icon elevation="2">
                        <v-icon> mdi-close </v-icon>
                     </v-btn>
                  </v-col>
               </v-row>
               <v-row>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('1')" x-large icon elevation="2">
                        <v-icon> mdi-numeric-1 </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('2')" x-large icon elevation="2">
                        <v-icon> mdi-numeric-2 </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('3')" x-large icon elevation="2">
                        <v-icon> mdi-numeric-3 </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('-')" x-large icon elevation="2">
                        <v-icon> mdi-minus </v-icon>
                     </v-btn>
                  </v-col>
               </v-row>
               <v-row>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('0')" x-large icon elevation="2">
                        <v-icon> mdi-numeric-0 </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('.')" x-large icon elevation="2">
                        <v-icon> . </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('=')" x-large icon elevation="2">
                        <v-icon> mdi-equal </v-icon>
                     </v-btn>
                  </v-col>
                  <v-col cols="3" class="d-flex justify-center">
                     <v-btn @click="click('+')" x-large icon elevation="2">
                        <v-icon> mdi-plus </v-icon>
                     </v-btn>
                  </v-col>
               </v-row>
            </v-card-text>
         </v-card>
      </v-dialog>
   </div>
</template>

<script>
export default {
   data: function () {
      return {
         dialog: false,
         calc: "",
      };
   },
   methods: {
      click: function (clicked) {
         const operators = ["×", "÷", "+", "-"];
         const numbers = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"];

         switch (clicked) {
            case "=":
               this.calculate();
               break;
            case "+":
               if (this.calc.length > 0) {
                  if (numbers.includes(this.calc[this.calc.length - 1])) {
                     this.calc += "+";
                  }
               }
               break;
            case "-":
               if (this.calc.length > 0) {
                  if (numbers.includes(this.calc[this.calc.length - 1])) {
                     this.calc += "-";
                  }
               } else {
                  this.calc += "-";
               }
               break;
            case "*":
               if (this.calc.length > 0) {
                  if (numbers.includes(this.calc[this.calc.length - 1])) {
                     this.calc += "×";
                  }
               }
               break;
            case "/":
               if (this.calc.length > 0) {
                  if (numbers.includes(this.calc[this.calc.length - 1])) {
                     this.calc += "÷";
                  }
               }
               break;
            case ".":
               let can_add_point = true;
               let operator_found = false;
               for (let i = this.calc.length - 1; i >= 0; i--) {
                  if (operator_found == false) {
                     if (operators.includes(this.calc[i])) {
                        operator_found = true;
                     } else if (this.calc[i] == ".") {
                        can_add_point = false;
                     }
                  }
               }
               if (can_add_point == true) {
                  this.calc += ".";
               }
               break;
            default:
               this.calc += clicked;
               break;
         }
      },
      calculate: function () {
         const charsAllowed = ["*", "/", "+", "-", ".", "0", "1", "2", "3", "4", "5", "6", "7", "8", "9"];
         const numbers = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"];

         let evaluate = this.calc;
         let clean = true;
         evaluate = evaluate.replaceAll("×", "*");
         evaluate = evaluate.replaceAll("÷", "/");
         if (evaluate.length <= 0) {
            clean = false;
         }
         [...evaluate].forEach((char) => {
            if (!charsAllowed.includes(char)) {
               clean = false;
            }
         });
         if (!numbers.includes(evaluate[evaluate.length - 1])){
            clean = false;
         } 
         if(clean){
            let res = eval(evaluate).toString();
            if(isNaN(res) || !isFinite(res) ){
               this.calc = "";
            } else {
               this.calc = res.toString();
            }
         }
      },
      erase: function () {
         this.calc = this.calc.slice(0, -1);
      },
      close: function () {
         this.dialog = false;
         this.calc = "";
      },
      show: function () {
         this.dialog = true;
      },
   },
};
</script>

<style></style>
