<template>
  <div>
    <div class="titlefont">
      <img src="./styles/cocinero.png">
      <h1 class="p-5">RECIPE BOOK</h1>
    </div>
    <div class="container">
      <CreateRecipe/>
    </div>
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
  font-family: 'Signika', sans-serif;
  font-size: 4rem; 
}
.body{
    background-color: #d4e2d4;
}
.titlefont{
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #fcf8e8;
  border-bottom-right-radius: 50%;
  border-bottom-left-radius: 50%;
  border-bottom: solid #df7861 5px;
}
.titlefont img{
  width: 100px; height: 100px;
}

@media (max-width: 740px) {
  .titlefont img{
    display:none;
  }
}

</style>
