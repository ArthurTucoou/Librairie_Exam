<template>
  <form @submit.prevent="$emit('critere', motcle)">
    <p>Rechercher des livres</p>
    <input
      class="btn"
      type="text"
      v-model="motcle"
      placeholder="Le critère"
      required
    />
    <input type="submit" value="Valider" class="btn" />
  </form>
</template>
<script setup>
import { ref } from "vue";
const motcle = ref("");
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

form input[type="text"] {
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