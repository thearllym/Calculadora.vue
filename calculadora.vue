<template>
  <div class="calculadora">
    <div class="display">{{current || '0' }}</div>
    <div v-on:click="limpar" class="btn">C</div>
    <div v-on:click="sinal" class="btn">+/-</div>
    <div v-on:click="porcentagem" class="btn">%</div>
    <div v-on:click="dividir" class="btn operacao">/</div>
    <div v-on:click="append('7')" class="btn">7</div>
    <div v-on:click="append('8')" class="btn">8</div>
    <div v-on:click="append('9')" class="btn">9</div>
    <div v-on:click="multiplicar" class="btn operacao">x</div>
    <div v-on:click="append('4')" class="btn">4</div>
    <div v-on:click="append('5')" class="btn">5</div>
    <div v-on:click="append('6')" class="btn">6</div>
    <div v-on:click="subtrair" class="btn operacao">-</div>
    <div v-on:click="append('1')" class="btn">1</div>
    <div v-on:click="append('2')" class="btn">2</div>
    <div v-on:click="append('3  ')" class="btn">3</div>
    <div v-on:click="somar" class="btn operacao">+</div>
    <div v-on:click="append('0')" class="btn zero">0</div>
    <div v-on:click="ponto" class="btn">.</div>
    <div v-on:click="igual" class="btn operacao">=</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      previous: null,
      current: '',
      operator: null,
      operatorClicked: false,
    };
  },
  methods: {
    up: function() {
    },
    limpar() {
      this.current = '';
    },
    sinal() {
      this.current = this.current.charAt(0) === '-'
        ? this.current.slice(1) 
        : `-${this.current}`;
    },
    porcentagem() {
      this.current = `${parseFloat(this.current) / 100}`
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`;
    },
    ponto() {
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
    SetPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    dividir() {
      this.operator = (a,b) => a / b;
      this.SetPrevious();
    },
    multiplicar () {
      this.operator = (a,b) => a * b;
      this.SetPrevious();
    },
    somar() {
      this.operator = (a,b) => a + b;
      this.SetPrevious();
    },
    subtrair() {
      this.operator = (a,b) => a - b;
      this.SetPrevious();
    },
    igual() {
      this.current = `${this.operator(
        parseFloat(this.current),
        parseFloat(this.previous)
      )}`;
      this.previous = null;
    }
  }
}
</script>

<style scoped>
.calculadora {
  margin: 0 auto;
  width: 400px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax (50px, auto);
}
 .display {
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
 }
 .zero {
   grid-column: 1 / 3;
 }
 .btn{
   background-color: #f2f2f2;
   border: 1px solid #999;
 }
 .operacao {
  background-color: orange;
  color: white;
}
</style>