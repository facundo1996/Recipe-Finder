<template>
    <div>
        <h2>Create new recipe</h2>
        <form>
          <!-- Recipe Name -->
          <div>
            <input  v-if="seen_input_name" type="text" placeholder="Name" id="input_name">
            <button v-if="visible_button" v-on:click="create_recipe">Create recipe</button>
            <p v-if="seen_p">{{recipe[0]}}</p>
          </div>

          <!-- Ingredients -->
          <div>
            <h3>Ingredients</h3>
            <input type="text" placeholder="Ingredient" id="input_ingredient">
            <button v-on:click="addIngredient" type="submit">Add ingredient</button>
            <ul>
              <li v-for="ingredient in ingredients">
                {{ingredient}}
              </li>
            </ul>
          </div>

          <!-- Sweet or salty -->
          <!-- <div>
              <div>
                <input type="radio" name="radio-group" id="Sweet" value="Sweet">
                <label for="js" class="radio-label">Sweet</label>
              </div>
              <div>
                <input type="radio" name="radio-group" id="Salty" value="Salty">
                <label for="tricky">Salty</label>
              </div>
          </div> -->

          <button v-on:click="addRecipe" type="submit">Add Recipe</button>

        </form>
    </div>
</template>

<script>
export default {
  name: 'CreateRecipe',
  data () {
    return{
      seen_input_name: true,
      visible_button: true,
      seen_p: false,
      recipe: [],
      ingredients:[],
      recipes: [],
    }
  },
  methods: {
    create_recipe(){
      
      if(input_name.value === ""){
        input_name.style.background="pink"
      }else{
        this.seen_input_name = false
        this.visible_button = false
        this.seen_p = true
        this.recipe.push(input_name.value)
        console.log(this.recipe)
      }
    },
    addIngredient(){
      this.ingredients.push(input_ingredient.value)
      input_ingredient.value = ""
      console.log(this.ingredients)
    },
    addRecipe(){
      this.recipe.push(this.ingredients)
      this.recipes.push(this.recipe)

      this.seen_input_name = true
      this.visible_button = true
      this.seen_p = false

      this.recipe=[]
      this.ingredients=[]

      console.log(this.recipes)
    }
    
  }
}

</script>

<style>

</style>