<template>
    <div class="div_CreateRecipe">
        <h2>Create new recipe</h2>
        <form>
          <!-- Recipe Name -->
          <div>
            <input  v-if="seen_input_name" v-model="value_recipe_name" type="text" placeholder="Recipe name" id="input_name">
            <button v-if="visible_button" v-on:click="create_recipe">Create recipe</button>
            <p v-if="seen_p">{{recipe[0]}}</p>
          </div>

          <!-- Ingredients -->
          <div>
            <h3>Ingredients</h3>
            <input type="text" v-model="value_ingredient" placeholder="Ingredient">
            <button v-on:click="addIngredient" type="submit">Add ingredient</button>
            <ul>
              <li v-for="ingredient in ingredients">
                {{ingredient}}
              </li>
            </ul>
          </div>

          <br>
          <br>

          <!-- Salty or Sweet -->
          <label :for="id + '_button'" :class="{'active': isActive}" class="toggle__button">
            <span class="toggle__label">{{ enableText }}</span>
            <input type="checkbox" :disabled="disabled" :id="id + '_button'" v-model="checkedValue">
            <span class="toggle__switch"></span>
            <span class="toggle__label">{{ disabledText }}</span>     
          </label>

          <br>
          <br>
          <input v-model="url_image" type="text" placeholder="Url Image">
          <button v-on:click="Add_image">Add image</button>
          <img :src="url_image">

          <br>
          <br>

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

      value_recipe_name :'',
      value_ingredient: '',

      /* Image */
      url_image:'',

      recipe: [],
      ingredients:[],
      recipes: [],


      // <!-- Salty or Sweet -->
      currentState: this.defaultState,
    }
  },
  props: {
    // <!-- Salty or Sweet -->
    disabled: {
      type: Boolean,
      default: false
    },
    labelEnableText: {
      type: String,
      default: 'Salty'
    },
    
    labelDisableText: {
      type: String,
      default: 'Sweet'
    },
    id: {
      type: String,
      default: 'primary'
    }, 
    defaultState: {
      type: Boolean,
      default: false
    }
  },

  // <!-- Salty or Sweet -->
    computed: {
        isActive() {
            return this.currentState;
        },
        enableText() {
            return this.labelEnableText;
        },
        disabledText() {
            return this.labelDisableText;
        },
        checkedValue: {
            get() {
                return this.currentState;
            },
            set(newValue) {
                this.currentState = newValue;
                this.$emit('change', newValue);
            }
        }
    },



  methods: {
    create_recipe(){
      if(this.value_recipe_name === ""){
        input_name.style.background="pink"
      }else{
        this.seen_input_name = false
        this.visible_button = false
        this.seen_p = true
        this.recipe.push(this.value_recipe_name)
        console.log(this.recipe)
      }
      console.log(this.valor_input)
    },
    addIngredient(){
      this.ingredients.push(this.value_ingredient)
      this.value_ingredient = ""
      console.log(this.ingredients)
    },
    Add_image(){
      console.log(this.url_image)

    },
    addRecipe(){
      this.recipe.push(this.ingredients)

      if (this.currentState === false) {
        this.recipe.push("Salty")
      }else{
        this.recipe.push("Sweet")
      }
      this.recipe.push(this.url_image)

      this.recipes.push(this.recipe)

      this.seen_input_name = true
      this.visible_button = true
      this.seen_p = false

      this.recipe=[]
      this.ingredients=[]
      console.log(this.recipes)

    },
    
  }
}

</script>

<style>
/* <!-- Salty or Sweet --> */
.toggle__button {
    vertical-align: middle;
    user-select: none;
    cursor: pointer;
}
.toggle__button input[type="checkbox"] {
    opacity: 0;
    position: absolute;
    width: 1px;
    height: 1px;
}
.toggle__button .toggle__switch {
    display:inline-block;
    height:12px;
    border-radius:6px;
    width:40px;
    background: #adedcb;
    box-shadow: inset 0 0 1px #53B883;
    position:relative;
    margin-left: 10px;
    transition: all .25s;
}
.toggle__button .toggle__switch::after, 
.toggle__button .toggle__switch::before {
    content: "";
    position: absolute;
    display: block;
    height: 18px;
    width: 18px;
    border-radius: 50%;
    left: 0;
    top: -3px;
    transform: translateX(0);
    transition: all .25s cubic-bezier(.5, -.6, .5, 1.6);
}
.toggle__button .toggle__switch::after {
    background: #53B883;
    box-shadow: 0 0 1px #53B883;
}
.toggle__button .toggle__switch::before {
    
    opacity:0;
}
.active .toggle__switch::after,
.active .toggle__switch::before{
  transform:translateX(40px - 18px);
}
.active .toggle__switch::after {
    left: 23px;
}




</style>