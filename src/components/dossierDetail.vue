<template>
  <div class="container my-5 p-5">
    <div class="d-flex justify-content-between align-items-center mb-3">
      <p class="m-0">Hi {{ data.first_name }} {{ data.last_name }}</p>
      <div class="d-flex gap-3 align-items-center">
        <button @click="deleteDossier">
          <i class="bi bi-trash3-fill"></i>
        </button>
        <a href="/edit">
          <i class="bi bi-pen-fill"></i>
        </a>
      </div>
    </div>
    <p class="">
      Your reservation token is : <b>{{ data.token }}</b>
    </p>
    <p class="">Status of your reservation : In progress</p>
  </div>
  <section class="d-flex justify-content-center">
    <button @click="clearSession" class="Logout">Done</button>
  </section>
</template>

<script>
import Cookies from "js-cookie";
import axios from "axios";

export default {
  name: "dossierPage",
  props: {
    data: {},
  },
  methods: {
    clearSession() {
      Cookies.remove("token");
      window.location.href = "/";
    },
    deleteDossier() {
      axios
        .delete(
          `http://localhost/myvisa/dossier/delete/${Cookies.get("token")}`
        )
        .then((response) => {
          if (response.data.message === "Dossier deleted") {
            this.clearSession();
            this.$router.push({ path: "/" });
          }
          console.log(response.data);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style scoped>
div {
  background-color: #2c3e50;
  color: white;
}
button {
  background-color: transparent;
  border: none;
  outline: none;
}
i {
  color: white;
}
.Logout {
  outline: 0;
  border: 0;
  padding: 10px;
  border-radius: 5px;
  background-color: #2c3e50;
  color: white;
}
</style>
