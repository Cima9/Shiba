<template>
  <div>
    <h1 style="color: purple">ShibaImages</h1>
    <div class="form-group">
      <label class="form-check" style="font-size: 25px"
        >Quanti shiba vuoi?</label
      >
      <input
        type="number"
        class=""
        v-model="inputUt"
        placeholder="Inserisci un numero"
      />
      <button class="btn btn-dark" @click="FetchApi()">Cerca</button>
    </div>
    <div v-if="response.length != 0">
      <div class="gallery">
        <div class="gallery-panel" v-for="item in response" :key="item">
          <img :src="item" />
          <button
            type="submit"
            style="border-radius: 0.5rem"
            class="btn btn-dark form-control"
            @click="SaveFoto(item)"
            data-toggle="modal"
            data-target="#staticBackdrop"
          >
            Save
          </button>
        </div>
      </div>
      <div
        class="modal fade"
        id="staticBackdrop"
        data-backdrop="static"
        data-keyboard="false"
        tabindex="-1"
        aria-labelledby="staticBackdropLabel"
        aria-hidden="true"
      >
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="staticBackdropLabel">
                Risposta dal server
              </h5>
              <button
                type="button"
                class="close"
                data-dismiss="modal"
                aria-label="Close"
              >
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <p :style="'color:' + Colore()">{{ response2 }}</p>
            </div>
            <div class="modal-footer">
              <button
                type="button"
                :class="'btn btn-' + Classe()"
                data-dismiss="modal"
              >
                Close
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Form",
  data() {
    return {
      inputUt: 0,
      response: [],
      response2: "",
    };
  },
  methods: {
    FetchApi() {
      console.log(this.inputUt);
      const axios = require("axios");
      axios
        .post("http://localhost:62852/Home/Shiba?numeroUt=" + this.inputUt)
        .then((response) => (this.response = response.data));
    },
    SaveFoto(item) {
      const axios = require("axios");
      console.log(item);
      axios
        .post("http://localhost:62852/Home/Shiba2?url=" + item)
        .then((response) => (this.response2 = response.data));
    },
    Colore() {
      if (this.response2 == "Aggiunta con successo! :)") return "green";
      else return "red";
    },
    Classe() {
      if (this.response2 == "Aggiunta con successo! :)") return "success";
      else return "danger";
    },
  },
};
</script>
<style>
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(20rem, 1fr));
  grid-gap: 1rem;
  max-width: 80rem;
  margin: 2rem auto;
  padding: 0 2rem;
}

.gallery-panel img {
  width: 100%;
  height: 22vw;
  object-fit: cover;
  border-radius: 0.75rem;
}
</style>