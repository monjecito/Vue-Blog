<template>
  <div class="general">
    <div class="center">
      <section id="content">
        <article class="article-item article-detail" v-if="article">
          <div class="image-wrap" v-if="article.image">
            <img
              :src="url + 'get-image/' + article.image"
              :alt="article.title"
            />
          </div>

          <h1 class="subheader">{{ article.title }}</h1>
          <span class="date"> {{ article.date | moment("from", "now") }} </span>
          <p>
            {{ article.content }}
          </p>

          <div class="clearfix"></div>

          <router-link :to="'/editar/' + article._id" class="btn btn-warning"
            >Editar</router-link
          >
          <a @click="deleteArticle(article._id)" class="btn btn-danger"
            >Eliminar</a
          >
        </article>
      </section>
      <Sidebar></Sidebar>
      <div class="clearfix"></div>
    </div>
  </div>
</template>
<script>
import Sidebar from "./Sidebar.vue";
import Global from "../Global";
import axios from "axios";
import swal from "sweetalert";
export default {
  name: "Article",

  components: {
    Sidebar,
  },

  data() {
    return {
      url: Global.url,
      article: null,
    };
  },
  mounted() {
    var articleId = this.$route.params.id;
    this.getArticle(articleId);
  },
  methods: {
    getArticle(articleId) {
      axios.get(this.url + "article/" + articleId).then((res) => {
        if (res.data.status == "success") {
          this.article = res.data.article;
        }
      });
    },
    deleteArticle(articleId) {
      swal({
        title: "Estas seguro?",
        text:
          "Una vez eliminado no podrás recuperarlos",
        icon: "warning",
        buttons: true,
        dangerMode: true,
      }).then((willDelete) => {
        if (willDelete) {
          axios.delete(this.url + "article/" +articleId).then((response) => {
            if (response) {
              swal({
                title: "Articulo borrado",
                text: "El articulo se ha editado correctamente",
                icon: "success",
              });
              this.$router.push("/blog");
            }
          });
        } else {
          swal("Al palo! No se pudo borrar");
        }
      });
    }
  }
};
</script>