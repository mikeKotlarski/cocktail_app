<template>
  <div>
    <div v-if="displayList == true">
      <br>
      <div
        class="alert"
        v-if="results.drinks == null"
      >Looks like there's no drinks out there by that name. Try another!</div>

      <div
        class="results"
        v-for="(drink,index) in results.drinks"
        @click="displayDrinkDetails(index);"
      >{{drink.strDrink}}</div>
    </div>
    <div v-if="displayDetail == true">
      <h4
        class="backButton"
        @click="displayDetail = false;
        displayList = true;"
      >< Back To Results</h4>
      <h2 class="title">{{results.drinks[drinkIndex].strDrink}}</h2>
      <h4>
        <div
          v-for="ingredient in ingredientList"
          v-bind:key="ingredient"
          class="ingredients"
        >{{ingredient}}</div>
      </h4>
      <p>
        <i>How to Enjoy:</i>
        <br>
        {{results.drinks[drinkIndex].strInstructions}}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "CocktaildbResults",
  props: {
    dbinput: {
      type: String
    }
  },
  data: function() {
    return {
      results: {},
      ingredientList: [],
      displayList: false,
      displayDetail: false,
      drinkIndex: null
    };
  },
  methods: {
    fetchAPIData(drinkName) {
      fetch(
        "https://www.thecocktaildb.com/api/json/v1/1/search.php?s=" + drinkName
      )
        .then(response => {
          if (response.ok) {
            return response.json();
          } else {
            alert(
              "Server returned " + response.status + " : " + response.statusText
            );
          }
        })
        .then(response => {
          console.log(response);
          this.results = response;
          this.displayList = true;
        })
        .catch(err => {
          console.log(err);
        });
    },
    generateIngredientList() {
      this.ingredientList = Object.values(
        Object.keys(this.results.drinks[this.drinkIndex])
          .filter(index => {
            return index.indexOf("strIngredient") == 0;
          })
          .reduce((ingredientList, index) => {
            ingredientList[index] = this.results.drinks[this.drinkIndex][index];
            return ingredientList;
          }, {})
      );
      console.log(this.ingredientList);
    },
    displayDrinkDetails(index) {
      this.drinkIndex = index;
      this.generateIngredientList();
      this.displayList = false; //clear screen
      this.displayDetail = true;
    }
  },
  watch: {
    dbinput: function() {
      this.fetchAPIData(this.dbinput);
    }
  }
};
</script>

<style scoped>
h2 {
  color: white;
  font-weight: 300;
}

h4 {
  margin-bottom: 0px;
  margin-top: 2px;
  padding-top: 0px;
  padding-bottom: 0px;
  font-family: Courier, sans-serif;
  font-weight: 100;
  color: white;
}

p {
  padding-top: 5px;
  padding-left: 10px;
  line-height: 1.4;
  font-family: palatino, serif;
  color: black;
}

.backButton {
  padding-top: 10px;
  padding-bottom: 0px;
  padding-left: 0px;
  color: black;
  font-weight: 300;
}

.backButton:hover {
  font-weight: bold;
}

.ingredients {
  padding-left: 10px;
}

.results {
  padding-left: 5px;
  padding-bottom: 2px;
  font-weight: 250;
}

.title {
  padding-left: 5px;
  margin-bottom: 2px;
}

.results:hover {
  font-weight: 350;
}

.alert {
  font-family: palatino, serif;
  font-weight: lighter;
  padding-left: 5px;
}
</style>
