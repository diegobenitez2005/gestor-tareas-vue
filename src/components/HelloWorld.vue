<template>
  <div class="hello">
    <h1>{{"App web de lista de tareas"}}</h1>
    <p>
      Esta es una página web sencilla creada con Vue. Puedes utilizarla como una lista de tareas y también recuperar tareas desde una API.
<br>
Puedes navegar por las distintas opciones que se encuentran en el menú de la cabecera.
      
    </p>
    
    
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
