<script>


export default {
  name: 'AppHeader',


}
</script>

<template>
  <header>

    <div class="container">
      
        <div>
          <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ3pGOvADo6HOMc-liAAdVtfDHAkkK1hycGIRtTewEn-g&s"
          alt="">
        </div>
        
        <div>
            <input type="text" v-model="searchQuery" @keyup.enter="searchAll"
              placeholder="Cerca un film o una serie TV..."> {{ " " }}
            <button @click="searchAll">Cerca</button>
        </div>

      

    </div>
  </header>
</template>



<style scoped>
.container {
  display: flex;
  justify-content: space-between; 
  align-items: center;
   
}

img {
  max-width: 100%;
}
</style>