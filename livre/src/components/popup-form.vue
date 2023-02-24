<template>
  <div class="popup-form">
    <form @submit.prevent="save">
      <div class="form-group">
        <label for="name">Titre:</label>
        <input type="text" id="name" v-model="name" required/>
      </div>
      <div class="form-group">
        <label for="price">Prix:</label>
        <input type="number" id="price" v-model="price" required/>
      </div>
      <div class="form-group">
        <label for="quantity">Quantité:</label>
        <input type="number" id="quantity" v-model="quantity" required/>
      </div>
      <div class="form-group">
        <button type="submit">Enregistrer</button>
      </div>
    </form>
    <button @click="this.$emit('close')">Annuler</button>
  </div>
</template>

<script>
import Livre from "@/Livre";

export default {
  data() {
    return {
      name: "",
      price: "",
      quantity: "",
    };
  },
  methods: {
    save() {
      let myHeaders = new Headers();
      myHeaders.append("content-type", "application/json");
      let leBody = JSON.stringify({ titre: this.name, qtestock: this.quantity, prix: this.price });
      console.log(leBody);
      const fetchOptions = {
        method: "POST",
        headers: myHeaders,
        body: leBody};
      fetch("https://webmmi.iut-tlse3.fr/~pecatte/librairies/public/20/livres", fetchOptions)
            .then(response => response.json())
            .then(json => {
              this.$emit("close");
            })
            .catch(error => {
              console.error(error);
            });
      }
    },
};
</script>

<style scoped>
.popup-form {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
  background-color: #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  border-radius: 8px;
}

.form-group {
  margin-bottom: 10px;
}

label {
  display: block;
  font-weight: bold;
}

input[type="text"] {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 8px 16px;
  cursor: pointer;
}
</style>