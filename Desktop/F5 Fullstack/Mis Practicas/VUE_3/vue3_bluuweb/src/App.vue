// /*17.06.23  https://youtu.be/PL-aTHv2L3E*/

<script setup>
import { FALSE } from 'sass';
import {ref, computed} from 'vue'; // Es del counter. counter ahora es una variable reactive y ya puede pasar a const. tenemos que hacer la importacion pq está modularizado, llamamos este paquete que tiene este poder de transformar una variable en una referencia reactiva. 
const name = "Vue dinámico";
const styleColor = "color: green";
const arrayColores = ["blue","red", "peru"];
const activo = null;

//v-for loop con array simple
const arrayFrutas = ["apple", "watermelon", "banana", "cherry",
"grapes"];

// V-for con un array de objetos 
const arrayCoches = [
  {
    name : "Mercedes",
    price : "50000",
    description : "Buen coche"
  },
  {
    name:"Volvo",
    price: "60000",
    description: "Mi coche preferido"
  },
  {
    name: "Seat",
    price: "20000",
    description: "Coche espanyol"
  }
];

//como recorrer un objeto:
const objetoVolvo = 
{
    name:"Volvo",
    price: "60.000 euros",
    description: "Mi coche preferido"
  } 

// v-if and v-for
const arrayCoches2 = [
  {
    name : "Mercedes",
    price : "50000",
    description : "Buen coche",
    stock: 0
  },
  {
    name:"Volvo",
    price: "60000",
    description: "Mi coche preferido",
    stock: 4
  },
  {
    name: "Seat",
    price: "20000",
    description: "Coche espanyol",
    stock: 5
  }
]; 

// metodos
//1 . sin parametro
const handleClick = () => {
  console.log('me diste click')
}

// 2. con parametro (mensaje)
const handleClickWithParam = (message) => {
  console.log(message);
};


//counter    ======= ref ========
//let counter = 0; //let y no const porque es variable. const no puede ser sobreescrita.pero si llamar (ref) de la biblioteca de Vue, sí puede ser const
const counter = ref(0); //importante inicializarlo. aquí es igual a cero. 
const increment = () => {
  counter.value ++;  //al pasar counter ser reactivo, anyadimos .value en script. pq Vue nos devuelve un objeto cual tiene propiedad .value. En tamplate no anyadimos .value. Lo accede automaticamente 
  console.log(counter);
};
//===Practica===
// const decrement = () => counter.value --;  O también 
const decrement = () => {
  counter.value --;
};

const reset = () =>  {
  counter.value = 0
}; // O bien    const reset = () =>  (counter.value = 0);

const classCounter = computed (() => { //No se puede hacer un computed sin un return explícido al contrario de la funcion de flecha  
  // --------Se recomienda utilizar computed cuando trabajamos con datos reactivos------//
  if(counter.value === 0 ){
    return 'zero';
  }
  if(counter.value > 0){
    return 'positive';
  }
  if(counter.value < 0){
    return 'negative';
  }
});


// ====Add button. Para acumular los números favoritos que está pasando usuario=====//
const arrayFavoritos = ref([]);
const add = () => {
  arrayFavoritos.value.push(counter.value);//counter.value es el numero actual que selecciono el usuario. como arrayFavoritos es una variable reactiva, tenemos que empujarla con .value
};

const bloquearBtnAdd = computed(()=> {
  const numSearch = arrayFavoritos.value.find(num => num === counter.value)
  //num === counter.value  es buscador del numero que ya existe en el array.
  console.log(numSearch);
  return numSearch ? true : false;
});
  
</script>



<template>
<br>
  <h1>Hello {{ name.toUpperCase() }}</h1>
  <h2> {{ arrayColores }}</h2>
  <h2 :style="`color: ${arrayColores[1]}`"> Soy {{arrayColores[1]}}</h2>
  <h2 :style="`color: ${arrayColores[0]}`">
   {{ activo ? "Estoy activo"  : "Estoy inactivo"}}
  </h2>
  <h2 v-if="activo=== true">Estoy Activo</h2>
  <p v-else-if=" activo === false">Estoy Inactivo</p>
  <p v-else>Estoy indeciso</p>

  =====   v-show para alternar algo con mucha frecuesncia =====
  <h2 v-show="activo">Estoy activo, Soy v-show</h2>


  ===== v-for loop  =====
  <ul>
    <!-- <li v-for="fruta in arrayFrutas">{{ fruta }}</li> -->
    <li v-for="(fruta, index) of arrayFrutas" 
        :key="index"> 
      {{ index }} - {{ fruta }}
    </li>
  </ul>

  =====V-for con un array de objetos =====
  <ul>
    <li v-for="coche in arrayCoches" :key="coche.name">
      {{ coche.name }} - {{ coche.price }} - {{ coche.description }}
    </li>
  </ul>

===== como recorrer un objeto.   parametros y key los elijo yo segun necesidad =====
<ul>
  <li v-for="(value, propiedad, index) in objetoVolvo" :key="index">
   {{ index+1 }} - {{ propiedad }} : {{ value }}

  </li>
</ul>

<!-- v-if and v-for . Template lo necesitamos solo para colocar v-for. en si mismo template sirvo de contenedor. no muestra nada especifico como ul, li, botton etc.  -->
=====v-if and v-for====
<template v-for="car in arrayCoches2" :key="car.name">
  <li v-if="car.stock > 0"> {{ car.name }} - {{ car.price }}</li>

</template>


<!-- metodos .   handleClick() es un metodo,pero si no tenemos paramtros a pasar, omitimos los "()""-->
<!-- v-on = @  -->
<button v-on:click="handleClick">Activame</button>
<button @click="handleClick">Activame2</button>
<button @click="handleClickWithParam('Soy un metodo con un mensaje (parametro)')">Click me</button>

<button @click.right.prevent="handleClickWithParam('Soy un click Right')">Click right</button>
<!-- .prevent es para prevenir que salga ventanilla de instrucciones al clic derecho del reton
Tembién hay .middle  para ratones con boton medio -->


<!-- counter -->
<!-- <h2 :class="counter > 0 ? 'positive' : 'negative'" >{{ counter }}</h2>    O mejor ;o siguiente, pq contiene color de zero tambien. Es más optimizado -->
<h2 :class="classCounter">{{ counter }}</h2>
<button @click="increment">Inrement the number</button>

 <!-- Practicas : descounter -->
 <button @click="decrement">Decrement the number</button>

 <!-- Reset Button -->
 <button @click="reset">Reset the counter</button>

 <!-- Add Button -->
 <button @click="add" :disabled="bloquearBtnAdd">Add</button>
 <br/>

 {{arrayFavoritos}}
 <ul>
  <!-- Este li está siguiendo al arrayFavoritos -->
  <li
  v-for="(num, index) in arrayFavoritos"
  :key="index"> 
    {{ num }}
  </li>
 </ul>

</template>





<style>
h1{
  color: pink;
}
.positive{
  color: green;
}
.negative{
  color: red;
}
.zero{
  color: turquoise;
}
</style>