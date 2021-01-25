<template>

    <div>
        <form>
            <input type="text" v-model="searcher_value" placeholder="Look for your recipe">
            <input type="submit" v-on:click="Search" value="Search">
        </form>
        <br>
        <fieldset>
            <label v-for="(search, index) in search_list">
                
                <input :id="index + '_id_search'" type="checkbox" v-on:click="Search_confirm(search,index)">
                {{search.name}}
                <button v-on:click="deleteT(search)">Delete</button>
                
            </label>
        </fieldset>
        
        <br>
        <!-- Salty or Sweet -->
        <label :for="id + '_button2'" :class="{'active': isActive}" class="toggle__button">
            <span class="toggle__label">{{ enableText }}</span>
            <input type="checkbox" :disabled="disabled" :id="id + '_button2'" v-model="checkedValue">
            <span class="toggle__switch"></span>
            <span class="toggle__label">{{ disabledText }}</span>     
        </label>

        <div v-if="search_list_confirm.length === 0">
            <div v-if="this.currentState === true">
                <!-- Sweet -->
                <div v-for="recipe in total_recipes">
                    <div v-if="recipe[2] === 'Sweet'">
                        <img :src="recipe[3]">
                        <p>{{recipe[0]}}</p>
                        <p>{{recipe[2]}}</p>
                        <span v-for="ingredient in recipe[1]">{{ingredient+","}}</span>
                        <hr>
                    </div>
                </div>
            </div>
            <div v-else>
                <!-- Salty -->
                <div v-for="recipe in total_recipes">
                    <div v-if="recipe[2] === 'Salty'">
                        <img :src="recipe[3]">
                        <p>{{recipe[0]}}</p>
                        <p>{{recipe[2]}}</p>
                        <span v-for="ingredient in recipe[1]">{{ingredient+","}}</span>
                        <hr>
                    </div>
                </div>
            </div>
        </div>
        <div v-else>
            <!-- Searcher activated -->
            <div v-if="this.currentState === true">
                <!-- Sweet -->
                <div v-for="recipe in list_found">
                    <div v-if="recipe[2] === 'Sweet'">
                        <img :src="recipe[3]">
                        <p>{{recipe[0]}}</p>
                        <p>{{recipe[2]}}</p>
                        <span v-for="ingredient in recipe[1]">{{ingredient+","}}</span>
                    </div>
                </div>
            </div>
            <div v-else>
                <!-- Salty -->
                <div v-for="recipe in list_found">
                    <div v-if="recipe[2] === 'Salty'">
                        <img :src="recipe[3]">
                        <p>{{recipe[0]}}</p>
                        <p>{{recipe[2]}}</p>
                        <span v-for="ingredient in recipe[1]">{{ingredient+","}}</span>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
export default {
    
    name: 'CreateRecipe',

    props:{
        total_recipes : {
            type: Array
        },

        boolean_search:{
            type: Boolean,
        },

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
  
    
    
    data(){
        return{
            searcher_value : '',
            search_list:[],
            search_list_confirm:[],
            valor: true,
            checkbox_search: Boolean,
            list_found: [],

            // <!-- Salty or Sweet -->
            currentState: this.defaultState,    
        }
    },
    methods:{
        deleteT(search){
            this.search_list.splice(this.search_list.indexOf(search), 1)
            this.search_list_confirm.splice(this.search_list_confirm.indexOf(search.name), 1)
        },
        Search(e){
            e.preventDefault()
            this.search_list.push({
                name: this.searcher_value,
                display: false
            })      
            this.searcher_value = ""
        },
        Search_confirm(search,index){
            if(document.getElementById(index+'_id_search').checked === true){
                this.list_found = []
                this.search_list_confirm.push(search.name)
            }
            else{
                this.list_found = []
                this.search_list_confirm.splice(this.search_list_confirm.indexOf(search.name), 1)
            }
            
            for(let i=0; this.total_recipes[i] ; i++){
                let add = false
                let addi = 0
                for(let x=0 ; this.search_list_confirm[x]; x++){
                    add = true
                    if(this.total_recipes[i][0].indexOf(this.search_list_confirm[x]) == -1){
                        console.log(this.search_list_confirm[x])
                        add = false
                        continue
                    }
                    if(add == true){
                        console.log("si sumo a ",this.search_list_confirm[x])
                        addi = addi + 1
                    }
                }
                for(let x=0 ; this.search_list_confirm[x]; x++){
                    for(let t = 0; this.total_recipes[i][1][t] ; t++){
                        if(this.total_recipes[i][1][t] == this.search_list_confirm[x]){
                            addi = addi + 1
                        }
                    }
                }
                if(addi == this.search_list_confirm.length){
                    this.list_found.push(this.total_recipes[i])
                }
            }
            
        },
        SearchInList(){

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
}
</script>
<style>
img{
    width: 150px;
}
</style>