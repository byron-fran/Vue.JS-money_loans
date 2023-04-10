
<script setup>
  import {ref, computed, watch} from 'vue'
  import Header from './components/Header.vue';
  import Button from './components/Button.vue';
  import {calcularAPagar} from '../helpers/app.js'


  const cantidad = ref(10000)
  const meses = ref(6)
  const total = ref(0)
//Variables 
  const MIN = 0;
  const MAX = 20000;
  const step = 100;
 function unaFuncion(){

 }
 
 unaFuncion()

  const formaterarDinero = ((valor)=>{
    const formater = new Intl.NumberFormat('en-US', {
      style: 'currency',
      currency : 'USD',
    })

    return formater.format(valor)
  })
  watch([cantidad, meses], ()=>{
    total.value = calcularAPagar(calcularAPagar(cantidad.value, meses.value))
  })

  const pagoMensual = computed(()=>{
    return total.value/meses.value
  })

  const handleChangeDecrement = ()=>{
    const valor = cantidad.value - step;
    if(valor< MIN){
      alert('Cantidad no válido')
      return
    }
    cantidad.value = valor
   }

  
  const handleChangeIncrement = ()=>{
    const valor = cantidad.value + step
    if(valor>MAX){
      alert('Cantidad no válido')
      return
    }
    cantidad.value = valor

  }
</script>

//Templates
<template>
  <div class="my-20 max-w-lg mx-auto shadow-md bg-white p-10">
    <Header></Header>
    <div class="flex justify-between mt-10">
     <Button :operador ="'-'" :fn = "handleChangeDecrement"></Button>
     <Button :operador="' +'" :fn = "handleChangeIncrement"></Button> 

    </div>

    <div class="mt-5">
      <input 
      type="range" 
      class="w-full bg-gray-200 accent-lime-500 hover:accent-lime-600"
      :min=" MIN"
      :max=" MAX"

      :step=" step"
      v-model.number="cantidad"
     />
    </div>
    <div>
      <p class="text-center text-indigo-600 font-extrabold text-5xl my-10">{{formaterarDinero(cantidad) }}</p>

      <h2 class="text-2xl font-extrabold text-center text-gray-600">Elige un <span class="text-indigo-600" >Plazo</span>A Pagar</h2>

      <select :value="meses" v-model.number="meses" class=" w-full bg-white border border-gray-300 rounded-lg text-center text-xl mt-12">
      <option value="6">6 Meses</option>
      <option value="9">9 Meses</option>
      <option value="12">12 Meses</option>
      </select>

    </div>
    <div v-if="total > 0" class="my-5 space-y-3 bg-white p-5" >
      <h2 class="text-2xl font-extrabold text-gray-500 text-center">Resumen <span class="text-indigo-600">De Pagos</span></h2>
      <p class="text-xl text-gray-500 text-center font-bold">{{ meses }} Meses</p>
      <p class="text-xl text-gray-500 text-center font-bold">{{formaterarDinero(total)}}Total A Pagar</p>
      <p class="text-xl text-gray-500 text-center font-bold">Mensualidades</p>
      <p class="text-xl text-gray-500 text-center font-bold">{{ formaterarDinero(pagoMensual) }}</p>

    </div>
    <p v-else class="text-center">Añade una cantidad</p>
  </div>

</template>

//Styles
<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
