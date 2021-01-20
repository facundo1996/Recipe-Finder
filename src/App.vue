<template>
  <div class="container">
    <h1>Recipe Finder</h1>
    <CreateRecipe/>
  </div>
</template>

<script>
import CreateRecipe from './components/CreateRecipe'

export default {
  name: 'App',
  components:{
    CreateRecipe
  }
}
</script>

<style>
h1{
  text-align: center;
}
</style>
