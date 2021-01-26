<template>
    <div class="mt-5 div_CreateRecipe">
        <h2 class="h2_h2"><u>Create new recipe</u></h2>
        <form class="form_createRecipe">
          <!-- Recipe Name -->
          <div>
            <h3 class="h3_h3">Recipe Name</h3>
            <input class="form-control mt-2 mb-3 font_input"  v-if="seen_input_name" v-model="value_recipe_name" type="text" placeholder="Recipe name..." id="input_name">
            <button class="btn btn_create" v-if="visible_button" v-on:click="create_recipe">Create recipe</button>
            <p class="name_recipe" v-if="seen_p">{{recipe[0]}}</p>
          </div>

          <!-- Ingredients -->
          <div>
            <h3 class="h3_h3 mt-5">Ingredients</h3>
            <input class="my-2 form-control font_input" type="text" v-model="value_ingredient" placeholder="Ingredient..." id="input_ingredient">
            <button class="btn btn_create" v-on:click="addIngredient" type="submit">Add ingredient</button>
            <ul class="list_ingredients">
              <li class="pt-2" v-for="ingredient in ingredients">
                {{ingredient}}
              </li>
            </ul>
          </div>

          <!-- Salty or Sweet -->
          <label :for="id + '_button'" :class="{'active': isActive}" class="toggle__button pb-4">
            <input type="checkbox" :disabled="disabled" :id="id + '_button'" v-model="checkedValue">
            <span class="Salty_Sweet toggle__label">{{ enableText }}</span><img class="mx-2" src="../styles/sal.png">
            <span class="toggle__switch"></span>
            <img class="mx-2" src="../styles/sugar.png">
            <span class="Salty_Sweet toggle__label">{{ disabledText }}</span>     
          </label>

          <div class="div_image">
            <input class="form-control font_input"  v-model="url_image" type="text" placeholder="Url Image...">
            <img class="mt-3" :src="url_image" id="images">
            <button class="btn btn_create" v-on:click="addRecipe" type="submit">Add Recipe</button>
          </div>

        </form>
        <RecipeFinder :total_recipes="recipes" />
    </div>
</template>

<script>
import RecipeFinder from './RecipesFinder'
export default {
  name: 'CreateRecipe',
  components:{
    RecipeFinder
  },
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
        this.value_recipe_name = ""
        console.log(this.recipe)
      }
      console.log(this.valor_input)
    },
    addIngredient(){
      if(this.recipe.length == 0){
        input_name.style.background="pink"
      }
      else if(this.value_ingredient.length == 0){
        input_ingredient.style.background="pink"
      }
      else{
        input_ingredient.style.background="white"
        this.ingredients.push(this.value_ingredient)
        this.value_ingredient = ""
      }
      
    },
    addRecipe(){
      if(this.recipe.length == 0){
        input_name.style.background="pink"
      }
      else{
        this.recipe.push(this.ingredients)

        if (this.currentState === false) {
          this.recipe.push("Salty")
        }else{
          this.recipe.push("Sweet")
        }
        this.recipe.push(this.url_image)
        this.url_image = ""

        this.recipes.push(this.recipe)

        this.seen_input_name = true
        this.visible_button = true
        this.seen_p = false

        this.recipe=[]
        this.ingredients=[]
        console.log(this.recipes)
      }

    },
    
  }
}

</script>

<style>
.h3_h3{
  font-family: 'Signika', sans-serif;
  font-size: 2rem;
  text-align: center;
}
.form_createRecipe{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.form_createRecipe div{
  display: flex;
  flex-direction: column;
  width: 300px;
}
.btn_create{
  background-color: #df7861;
  font-family: 'Rubik', sans-serif;
}
.btn_create:hover{
  background-color: #ecb390;
}
.name_recipe{
  text-align: center;
  font-size: 30px;
  font-family: 'Rubik', sans-serif;
  font-weight: bold;
}
.list_ingredients{
  margin-left: 20px;
  margin-top: 10px;
  font-family: 'Rubik', sans-serif;
  font-size: 25px;
  list-style: none;
}
.list_ingredients li{
  background-image: url(../styles/zanahoria.png);
  background-size: 25px;
  background-position: left center;
  background-repeat: no-repeat;
  text-align: center;
  padding-bottom: 5px;
}
.div_image{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.div_image button{
  width: 300px;
  margin-top: 20px;
}
.font_input{
  font-family: 'Rubik', sans-serif;
}



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

.titlefont{
  font-family: 'Signika', sans-serif;
  
}
.Salty_Sweet{
  font-size: 20px;
  font-family: 'Rubik', sans-serif;

}
.toggle__button img{
  width: 35px;
  height: 35px;
}



</style>