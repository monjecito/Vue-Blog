<template>
  <div class="general">
    <Slider texto="Pagina de pruebas"></Slider>
    <div class="center">
      <section id="content">
        <h2 class="subheader">PAGINA DE PRUEBAS</h2>
        <h3>{{ id_desde_url }}</h3>
        <button @click="actualizarNombre()">Actualizar nombre</button>
        <button @click="redirigirBlog()">Redirigir al Blog</button>
        <h4>{{ nombre }}</h4>
      </section>
      <Sidebar></Sidebar>
      <div class="clearfix"></div>
    </div>
  </div>
</template>

<script>
import Slider from "./Slider.vue";
import Sidebar from "./Sidebar.vue";
export default {
  name: "Pagina",
   components: {
      Slider,
      Sidebar,
    },
  created() {
    console.log("Componente creado");
  },
  mounted() {
    this.id_desde_url = this.$route.params.id; //OBTENER PARAMETROS POR LA RUTA
    console.log("Componente cargado");
  },
  updated() {
    console.log("Componente actualizado");
  },
  destroyed() {
    console.log("Componente destruido");
  },
  data() {
    return {
      id_desde_url: null,
      nombre: "Diego Monje",
    };
  },
  methods: {
    actualizarNombre() {
      this.nombre = "DIEGO MONJE EL CRACK";
      console.log(this.nombre);
    },
    redirigirBlog() {
      this.$router.push("/blog");
    },
   
  },
};
</script>