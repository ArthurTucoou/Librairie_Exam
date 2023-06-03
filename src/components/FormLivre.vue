<template>
  <form @submit.prevent="$emit('addl', livre)">
    <p>Ajouter un livre</p>
    <input
      class="btn"
      type="text"
      v-model="livre.titre"
      placeholder="Le titre"
      required
    />
    <input
      class="btn"
      type="number"
      v-model="livre.qtestock"
      placeholder="La quantité"
      min="1"
      required
    />
    <input
      class="btn"
      type="number"
      step="any"
      v-model="livre.prix"
      placeholder="Le prix"
      min="1"
      required
    />

    <input type="submit" value="Valider" class="btn" />
  </form>
</template>
<script setup>
import { reactive } from "vue";
import Livre from "../Livre.js";
const livre = reactive(Livre);
</script>

<style scoped>
form {
  width: 300px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

form input[type="text"],
form input[type="number"] {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  box-sizing: border-box;
}

form input[type="submit"] {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 8px 16px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s;
}

form input[type="submit"]:hover {
  background-color: #45a049;
}
</style>