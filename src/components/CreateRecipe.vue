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
            <input type="text" placeholder="Ingredient">
            <button type="submit">Add ingredient</button>
          </div>

          <!-- Sweet or salty -->
          <div>
              <div>
                <input type="radio" name="radio-group" id="Sweet" value="Sweet">
                <label for="js" class="radio-label">Sweet</label>
              </div>
              <div>
                <input type="radio" name="radio-group" id="Salty" value="Salty">
                <label for="tricky">Salty</label>
              </div>
          </div>

          <button type="submit">Add Recipe</button>

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
      
    }

  }
}

</script>

<style>

</style>