<template>

  <main class="dashboard">
<div>
<p>Puntaje</p><p id="puntaje"></p>
</div>
    <div class="selected-card">
    <Card :character="chosenCharacter" :closed="isClosed" 
     @click="reinicio()"
     />
    </div>

    <div class="options">
    <Card 
    v-for="option in options" 
    :key="option.id" 
    :character="option" 
    :closed="false" 
    @click="validate(option.id)"
/>
</div>

  </main>
</template>
<script>
  let contador =0;
    import { getCharacters, randomNumber } from "./utils/api";
    import Card from "./components/Card";

    export default {

        name: "App",

        components: {
            Card
        },
        data() {
    return {
        options: [],
        chosenCharacter: {},
        isClosed: true
    }
},
        
        async beforeCreate() {
    
const characters = await getCharacters();
    const chosenCharacter = characters[ randomNumber(0, 2) ];
    this.options=characters;
    this.chosenCharacter = chosenCharacter;
},
methods: {
  async reinicio(){
    this.isClosed = true;
const characters = await getCharacters();
    const chosenCharacter = characters[ randomNumber(0, 2) ];
    this.options=characters;
    this.chosenCharacter = chosenCharacter;
    
  },
  count(contador){
  contador++
  document.querySelector("#puntaje").innerHTML=contador;
  return contador
},

    validate(chosenCharacter) {

        if (chosenCharacter === this.chosenCharacter.id) {
        this.isClosed = false;
        alert("¡Lo has adivinado! Presiona la carta de arriba para volver a empezar");
        contador=this.count(contador)
        
       
    }
    else {
        alert("Personaje incorrecto, inténtalo de nuevo");
    }

}


    }

}

    

</script>
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: Arial, Helvetica, sans-serif;
}
p{
  color:#023047;
  font-size:25px
}
.dashboard {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 20px;
  background: rgb(154,221,212);
  background: linear-gradient(114deg, rgba(154,221,212,1) 0%, rgba(171,214,227,1) 100%);
}
.image-container {
  display: flex;
}
.image-container img {
  display: inline-block;
  width: 100%;
}
.selected-card {
  display: flex;
  justify-content: center;
}
.options {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  margin-top: 2rem;
  gap: 15px;
  justify-items: center;
}
.options .card {
  cursor: pointer;
}
</style>