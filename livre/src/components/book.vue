<script>
import Livre from "@/Livre";

export default {
  name: "book",
  props: {
    livre: Object
  },
  computed: {
    getLivre() {
      return this.livre;
    },
  }
}
</script>

<template>
  <tr>
    <td id="img">
      <img   :src="`/src/components/img/couverture-1.png`" alt="Cover Image">
    </td>
    <td>{{getLivre.titre }}</td>
    <td>{{getLivre.qtestock }}</td>
    <td>{{getLivre.prix }} $</td>
    <td id="actions">
      <button id="delete" @click="this.$emit('delete', getLivre.id)">Supprimer</button>
      <button id="addQ" @click="this.$emit('addQ', getLivre)">+</button>
      <button id="removeQ" @click="this.$emit('removeQ', getLivre)">-</button>
    </td>
  </tr>
</template>

<style scoped>
/* Table cell style */
td {
  border: 1px solid #ddd;
  padding: 10px;
  min-width: 33%;
  text-align: center;
}

button {
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 8px 16px;
  cursor: pointer;
  margin-right: 2vh;
}

button#delete {
  background-color: red;
}

button#addQ {
  background-color: green;
}

button#removeQ {
  background-color: orange;
}

img{
  width: 20vh;
}
</style>