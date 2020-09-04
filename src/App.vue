/*
Cocktail, an App
Version 1

Will fetch API data from thecocktaildb, and provide a seprate detailed pages
for each drink, including ingredients, pictures, and instructions.

bon appertif!
*/

<template>
  <div id="app">
    <h1 class="appTitle">Cocktail, an App</h1>
    <TextInput v-on:inputData="setDrinkName"/>
    <CocktaildbResults v-bind:dbinput="drinkName"/>
  </div>
</template>

<script>
import TextInput from "./components/TextInput";
import CocktaildbResults from "./components/CocktaildbResults";

export default {
  name: "cocktailApp",
  components: {
    TextInput,
    CocktaildbResults
  },

  data: function() {
    return {
      drinkName: ""
    };
  },

  methods: {
    setDrinkName(input) {
      this.drinkName = input;
    }
  }
};
</script>

<style>
body {
  background-color: darkgrey;
  font-family: Gill Sans, sans-serif;
}

h1 {
  font-weight: 400;
  color: white;
  margin-bottom: 10px;
}
</style>
