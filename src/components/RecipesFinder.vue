<template>
    <div>
        <input type="text" v-model="searcher_value" placeholder="Look for your recipe">
        <button v-on:click="Search">Search</button>
        <div>
            <div v-for="recipe in total_recipes">
                <img :src="recipe[3]">
                <p>{{recipe[0]}}</p>
                <p>{{recipe[2]}}</p>
                <span v-for="ingredient in recipe[1]">{{ingredient+","}}</span>
                <hr>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CreateRecipe',
    props:['total_recipes'],
    data(){
        return{
            searcher_value : ''
        }
    },
    methods:{
        Search(){
            console.log(this.searcher_value)
        }
    }
}
</script>
<style>
img{
    width: 150px;
}

</style>