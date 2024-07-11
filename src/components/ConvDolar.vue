<template>
  <div>
    <h1>Conversor de pesos a dólar</h1>
    <hr>
    <p>Ingrese cantidad de pesos <input type="text" v-model.number="pesos"></p>
    <p>Valor del dólar en pesos <input type="text" v-model.number="dolar">
      - Actualización <input type="checkbox" v-model="actualizacion" @click="actualizar()"><span v-if="actualizarActivado"> ultima actualizacion {{ this.cotizaciones.last_update }}</span></p>
    <p>Valor convertido USD: {{ valorConvertido }}</p> 
   
  </div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'src-componentes-conv',
  props: [],
  data() {
    return {
      pesos: '',
      dolar: '',
      cotizaciones: {},
      actualizarActivado: false,
    };
  },
  mounted() {
    this.getCotizaciones();

  },
  methods: {
    getValor(pesos, dolar) {
      let valor = null;
      if (pesos && dolar) {
        valor = pesos / dolar;
      }
      return valor;
    },
    async getCotizaciones() {
      const response = await axios.get('https://api.bluelytics.com.ar/v2/latest')
        this.cotizaciones = response.data
        this.dolar = response.data.blue.value_sell
        
    },
    actualizar(){
      if(!this.actualizarActivado){
        this.actualizarActivado = true
        
      }else{
        this.actualizarActivado = false
      }
    },
    actualizarValor(){
      this.getCotizaciones()
    }
  },
  computed: {
    valorConvertido() {
      const valor = this.getValor(this.pesos, this.dolar);
      return valor !== null ? valor.toFixed(2) : null;
    },
  },
};
</script>

<style scoped lang="css">
</style>
