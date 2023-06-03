<template>
  <div class="forms">
    <SearchLivres class="formSearch" @critere="searchLivres" />
    <FormLivre class="formAdd" @addl="addLivre" />
  </div>

  <h3>Liste des livres</h3>
  <ul class="livres">
    <li class="livre" v-for="livre in listeLivres" :key="[livre.id]">
      <h4>{{ livre.titre }}</h4>
      <div style="align-items: center; display: flex; justify-content: center">
        <p style="padding: 10px">En stock : {{ livre.qtestock }}</p>
        <div style="display: flex">
          <button @click="ajouterLivre(livre)" class="btn">+</button>
          <button
            @click="
              livre.qtestock == 1 ? deleteLivre(livre.id) : retirerLivre(livre)
            "
            class="btn"
          >
            -
          </button>
        </div>
      </div>
      <div>
        <b> {{ livre.prix }} € </b>
      </div>

      <button @click="deleteLivre(livre.id)" class="btn">Supprimer</button>
    </li>
  </ul>
</template>

<script setup>
import { reactive, watch, defineProps } from "vue";
import Livre from "../Livre.js";
import FormLivre from "./FormLivre";
import SearchLivres from "./SearchLivres";
const props = defineProps(["critere"]);
let url = "https://webmmi.iut-tlse3.fr/~pecatte/librairies/public/4/livres";
const listeLivres = reactive([]);

watch(props, (newprops) => {
  searchLivres(newprops.critere);
});

function searchLivres(motcle) {
  //Si un critère est renseigné
  if (motcle) {
    url =
      "https://webmmi.iut-tlse3.fr/~pecatte/librairies/public/4/livres?search=" +
      motcle;
  }
  const fetchOptions = { method: "GET" };
  fetch(url, fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
      console.log(dataJSON);
      listeLivres.splice(0, listeLivres.length);
      dataJSON.forEach((livre) => {
        listeLivres.push(
          new Livre(livre.id, livre.titre, livre.qtestock, livre.prix)
        );
      });
      console.log(listeLivres);
    })
    .catch((error) => {
      console.log(error);
    });
}

function addLivre(l) {
  let myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");
  const fetchOptions = {
    method: "POST",
    headers: myHeaders,
    body: JSON.stringify({
      titre: l.titre,
      qtestock: l.qtestock,
      prix: l.prix,
    }),
  };
  fetch(url, fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
      console.log(dataJSON);
      searchLivres();
    })
    .catch((error) => {
      console.log(error);
    });
}

function deleteLivre(index) {
  const fetchOptions = { method: "DELETE" };
  fetch(url + "/" + index, fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
      console.log(dataJSON);
      searchLivres();
    })
    .catch((error) => {
      console.log(error);
    });
}

function ajouterLivre(livre) {
  let myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");
  const fetchOptions = {
    method: "PUT",
    headers: myHeaders,
    body: JSON.stringify({
      id: livre.id,
      titre: livre.titre,
      qtestock: livre.qtestock + 1,
      prix: livre.prix,
    }),
  };
  fetch(url, fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
      console.log(dataJSON);
      searchLivres();
    })
    .catch((error) => {
      console.log(error);
    });
}

function retirerLivre(livre) {
  let myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");
  const fetchOptions = {
    method: "PUT",
    headers: myHeaders,
    body: JSON.stringify({
      id: livre.id,
      titre: livre.titre,
      qtestock: livre.qtestock - 1,
      prix: livre.prix,
    }),
  };
  fetch(url, fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
      console.log(dataJSON);
      searchLivres();
    })
    .catch((error) => {
      console.log(error);
    });
}

searchLivres();
</script>

<style scoped>
h3 {
  text-align: center;
  color: #333;
}

.forms {
  display: flex;
}

.livre {
  width: 15rem;
  list-style-type: none;
  margin: 15px;
  padding: 10px;
  border: 1px solid #ddd;
  background-color: #f9f9f9;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
}

.livre:hover {
  transform: scale(1.05);
}

.livres {
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.btn {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 8px 16px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #45a049;
}
</style>