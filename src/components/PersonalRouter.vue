<template>
  <router-link :to="route">{{ buttonText }}</router-link>
</template>

<script setup>


defineProps({
  route: String,
  buttonText: String,
});

// Router: Define las rutas
// router - link: me dirige al componente.como un tag <a>.
// router-link -> Carga y renderiza el componente asociado para mostrarlo por una salida de router-view por defecto o por la salida con su mismo nombre.

// router-view: me enseña el componente.


</script>

<style></style>
