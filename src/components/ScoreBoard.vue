<template>

<section class="score">
      Jogador <span>{{contador}}</span> x 
     <span>{{ pontosComputador }} </span> Computador
    </section>

    <template v-if="this.question">
      <h1 v-html="this.question">
      </h1>
    </template>
    
</template>

<script>

export default{
    props:
        [
            'contador',
            'pontosComputador'
        ]
    
}



</script>



<style  lang="scss">

section.score{
    border-bottom:1px solid black;
    padding: 24px;
    font-size: 18px;
    span{
        padding: 8px;
        font-weight: bold;
        border:1px solid black;
    }
}

</style>