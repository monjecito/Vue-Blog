<template src="./CreateArticle.html"></template>


<script>
import Sidebar from "./Sidebar.vue";

import Article from "../models/Article";
import axios from "axios";
import Global from "../Global";
import swal from "sweetalert";

export default {
  name: "Create-Article",

  components: {
    Sidebar,
  },
  data() {
    return {
      file: "",
      article: new Article("", "", null, ""),
      url: Global.url,
    };
  },
  mounted() {
    //console.log(this.article);
  },

  methods: {
    fileChange() {
      this.file = this.$refs.file.files[0];
      console.log(this.file);
    },
    save() {
      axios
        .post(this.url + "save", this.article)
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
                .post(this.url + "/upload-image/" + articleId, formData)
                .then((response) => {
                  if (response.data.article) {
                    swal({
                      title: "Articulo creado",
                      text: "El articulo se ha creado correctamente",
                      icon: "success",
                    });
                    this.article = response.data.article;
                    this.$router.push("/blog");
                  } else {
                    //Alerta de error
                    swal({
                      title: "Articulo fallido",
                      text: "El articulo no se pudo crear",
                      icon: "error",
                    });
                  }
                })
                .catch((err) => {
                  console.log(err);
                });
            } else {
              swal({
                      title: "Articulo creado",
                      text: "El articulo se ha creado correctamente",
                      icon: "success",
                    });
              this.article = response.data.article;
              this.$router.push("/blog");
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