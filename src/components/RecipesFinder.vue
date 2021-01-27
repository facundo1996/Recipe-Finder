<template>

    <div class="div_recipe_finder">
        <div class="div_recipe_finder_1">
            <form class="d-flex">
                <input class="form-control font_input" id="input_search" type="text" v-model="searcher_value" placeholder="Look for your recipe...">
                <input class="btn btn_create mx-3" type="submit" v-on:click="Search" value="Search">
            </form>

            <!-- Salty or Sweet -->
            <label :for="id + '_button2'" :class="{'active': isActive}" class="toggle__button">
                <span class="Salty_Sweet toggle__label">{{ enableText }}</span><img class="mx-2" src="../styles/sal.png">
                <input type="checkbox" :disabled="disabled" :id="id + '_button2'" v-model="checkedValue">
                <span class="toggle__switch"></span>
                <img class="mx-2" src="../styles/sugar.png"><span class="Salty_Sweet toggle__label">{{ disabledText }}</span>      
            </label>
        </div>
        
        <div v-if="search_list.length" class="div_fielset">    
            <fieldset class="my-5 fieldset">
                <label class="mx-2 my-2" v-for="(search, index) in search_list">
                    <input :id="index + '_id_search'" type="checkbox" v-on:click="Search_confirm(search,index)">
                    {{search.name}}
                    <img class="img_x" src="../styles/x.png" v-on:click="deleteT(search)">
                </label>
            </fieldset>
        </div>
        

        <div class="div_cards" v-if="search_list_confirm.length === 0">
            <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5" v-if="this.currentState === true">
                <!-- Sweet -->
                <div v-for="recipe in total_recipes" v-if="recipe[2] === 'Sweet'">
                    <div class="card_div1 col mb-4">
                        <div class="card_div card h-100">
                            <img class="card-img-top" style="height: 150px;" :src="recipe[3]">
                            <div class="card-body">
                                <p class="card_name">{{recipe[0]}}</p>
                                <p class="card_sa_sw">{{recipe[2]}}</p>
                                <p class="card_ingredient" v-html="recipe[1].join()"></p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div v-else class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5">
                <!-- Salty -->
                <div v-for="recipe in total_recipes" v-if="recipe[2] === 'Salty'">
                    <div class="card_div1 col mb-4">
                        <div class="card_div card h-100">
                            <img class="card-img-top" :src="recipe[3]">
                            <div class="card-body">
                                <p class="card_name">{{recipe[0]}}</p>
                                <p class="card_sa_sw">{{recipe[2]}}</p>
                                <p class="card_ingredient" v-html="recipe[1].join()"></p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="div_cards" v-else >
            <!-- Searcher activated -->
            <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5" v-if="this.currentState === true">
                <!-- Sweet -->
                <div v-for="recipe in list_found" v-if="recipe[2] === 'Sweet'">
                    <div class="card_div1 col mb-4">
                        <div class="card_div card h-100">
                            <img class="card-img-top" :src="recipe[3]">
                            <div class="card-body">
                                <p class="card_name">{{recipe[0]}}</p>
                                <p class="card_sa_sw">{{recipe[2]}}</p>
                                <p class="card_ingredient" v-html="recipe[1].join()"></p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div v-else class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5">
                <!-- Salty -->
                <div v-for="recipe in list_found" v-if="recipe[2] === 'Salty'">
                    <div class="card_div1 col mb-4">
                        <div class="card_div card h-100">
                            <img class="card-img-top" :src="recipe[3]">
                            <div class="card-body">
                                <p class="card_name">{{recipe[0]}}</p>
                                <p class="card_sa_sw">{{recipe[2]}}</p>
                                <p class="card_ingredient" v-html="recipe[1].join()"></p>
                            </div>
                        </div>
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
				},
    },

    data(){
        return{
            searcher_value : '',
            search_list:[],
            search_list_confirm:[],
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
            if(this.searcher_value == ""){
                input_search.style.background="pink"
                }
            else{
                e.preventDefault()
                this.search_list.push({
                    name: this.searcher_value,
                    display: false
                })      
                this.fielset_display = true
                this.searcher_value = ""
                input_search.style.background="white"
            }
        },
        Search_confirm(search,index){
            if(document.getElementById(index+'_id_search').checked){
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
                        add = false
                        continue
                    }
                    if(add == true){
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
.div_recipe_finder{
    display: flex;
    flex-direction: column;
}
.div_recipe_finder_1{
    display: flex;
    justify-content: space-around;
}
.div_fielset{
    display: flex;
    justify-content: center;
}
.fieldset{
    background-color: rgba(245, 244, 244, 0.83);
    padding: 20px 30px;
    border-radius: 60px;
}
.fieldset label{
    background-color: rgba(158, 158, 158, 0.83);    
    padding: 8px 8px;
    border-radius: 60px;
    font-family: 'Rubik', sans-serif;
}
.img_x{
    width: 25px;
    height: 25px;
    border-radius: 100%;
    background-color: rgba(245, 244, 244, 0.83);
    padding: 5px;
}
.img_x{
    cursor: pointer;
}

.card_div1{
    width: 220px;
    height: 75%;
    position: relative;
    
}
.card_div{
    padding-bottom: 50px;
    background-color: #fcf8e8;
    margin-top: 80px;
    border: solid 1px #ecb390;
    
}
.card_div img{
    width: 100%;
    height: 120px !important;
    object-fit: cover;
   
}
.card_name{
    font-family: 'Signika', sans-serif;
    font-size: 20px;
}
.card_sa_sw{
    font-family: 'Rubik', sans-serif;
}
.card_ingredient{
    font-family: 'Rubik', sans-serif;
}

@media (max-width: 770px){
    .div_recipe_finder_1{
        display: flex;
        flex-direction: column;
        justify-content: center;
        text-align: center;
    }
    .div_recipe_finder_1 form{
        margin-bottom: 40px;
    }
}
@media (max-width: 580px){
    .card_div1{
        margin-left: 27%;
    }
}


img{
    width: 200px;
}
</style>