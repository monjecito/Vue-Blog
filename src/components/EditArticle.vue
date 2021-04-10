<template src="./CreateArticle.html"></template>


<script>
import Sidebar from "./Sidebar.vue";

import Article from "../models/Article";
import axios from "axios";
import Global from "../Global";
import swal from "sweetalert";

export default {
  name: "EditArticle",

  components: {
    Sidebar,
  },
  data() {
    return {
      file: "",
      article: new Article("", "", null, ""),
      url: Global.url,
      isEdit: true,
    };
  },
  mounted() {
    //console.log(this.article);
    var articleId = this.$route.params.id;
    this.getArticle(articleId);
  },

  methods: {
    fileChange() {
      this.file = this.$refs.file.files[0];
      console.log(this.file);
    },

    getArticle(articleId) {
      axios.get(this.url + "article/" + articleId).then((res) => {
        if (res.data.status == "success") {
          this.article = res.data.article;
        }
      });
    },
    save() {
      var articleId = this.$route.params.id;
      axios
        .put(this.url + "article/" + articleId, this.article)
        .then((response) => {
          if (response.data.status == "success") {
            //Subida de archivo
            if (
              this.file != null &&
              this.file != undefined &&
              this.file != ""
            ) {
              const formData = new FormData();
              formData.append("file0", this.file, this.file.name);
              var articleId = response.data.article._id;
              axios
                .post(this.url + "upload-image/" + articleId, formData)
                .then((response) => {
                  if (response.data.article) {
                    swal({
                      title: "Articulo editado",
                      text: "El articulo se ha editado correctamente",
                      icon: "success",
                    });
                    this.article = response.data.article;
                    this.$router.push("/articulo/"+this.article._id);
                  } else {
                    //Alerta de error
                    swal({
                      title: "Articulo fallido",
                      text: "El articulo no se pudo editar",
                      icon: "error",
                    });
                  }
                })
                .catch((err) => {
                  console.log(err);
                });
            } else {
              swal({
                title: "Articulo editado",
                text: "El articulo se ha editado correctamente",
                icon: "success",
              });
              this.article = response.data.article;
              this.$router.push("/articulo/"+this.article._id);
            }
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>