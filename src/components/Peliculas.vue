<template>
  <div class="general">
    <div class="center">
      <section id="content">
        <h1 class="subheader">POLLA FRITA</h1>
        <div class="mis-datos" v-if="misDatos">
          {{ misDatos | mayusculas |concatenar('Este es el puto mejor año')}}
        </div>
        <div class="favorita" v-if="favorita">
          <h2>La serie favorita es : {{ favorita.title }}</h2>
        </div>

        <div id="articles">
          <!--  -->
          <!--AÑADIR ARTICULOS VIA JS-->
          <div
            v-for="pelicula in peliculasMayuscula"
            v-bind:key="pelicula.title"
          >
            <Pelicula
              :pelicula="pelicula"
              @favorita="haLlegadoLaPeliculaFavorita"
            ></Pelicula>
          </div>
        </div>
      </section>
      <Sidebar></Sidebar>
      <div class="clearfix"></div>
    </div>
  </div>
</template>


<script>
import Sidebar from "./Sidebar.vue";
import Pelicula from "./Pelicula.vue";
export default {
  name: "Series",
  components: {
    Pelicula,
    Sidebar,
  },
  methods: {
    haLlegadoLaPeliculaFavorita(favorita) {
      this.favorita = favorita;
    },
  },

  filters: {
    mayusculas(value) {
      return value.toUpperCase();
    },
    concatenar(value,message){
     var date=new Date();
     return value+ ' '+date.getFullYear()+' '+message;
    }
  },
  computed: {
    peliculasMayuscula() {
      var peliculasMod = this.peliculas;
      for (var i = 0; i < this.peliculas.length; i++) {
        peliculasMod[i].title = peliculasMod[i].title.toUpperCase();
      }

      return peliculasMod;
    },

    misDatos() {
      return this.nombre + " " + this.apellidos;
    },
  },

  data() {
    return {
      nombre: "Diego",
      apellidos: "Monje",
      favorita: null,
      peliculas: [
        {
          title: "Breaking bad",

          year: 2008,
          image:
            "https://imagenes.20minutos.es/files/article_amp/uploads/2019/10/22/breakingBad.jpg",
        },
        {
          title: "Bojack Horseman",

          year: 2015,
          image:
            "https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/bojack-horseman-final-1571059533.jpg?crop=0.752xw:1.00xh;0.126xw,0&resize=640:*",
        },
        {
          title: "Dark",

          year: 2013,
          image:
            "https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/dark-temporada-3-fecha-estreno-netflix-1590478686.jpeg",
        },
        {
          title: "Desencanto",

          year: 2018,
          image:
            "https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/desencanto-1527089594.jpg?crop=1.00xw:0.433xh;0,0.349xh&resize=480:*",
        },
      ],
    };
  },
};
</script>