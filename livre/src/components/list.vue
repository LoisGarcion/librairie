<script>
  import book from "@/components/book.vue";
  import popupForm from "@/components/popup-form.vue";
  import Livre from "@/Livre.js";
  import {reactive} from "vue";
  export default {
    name: "list",
    components: {
      book,
      popupForm
    },
    data() {
      return {
        list: reactive([]),
        showingPopup: false,
        search: '',
      }
    },
    methods: {
      getLivres() {
        let url;
        if(this.search !== ''){
          url = "https://webmmi.iut-tlse3.fr/~pecatte/librairies/public/20/livres?search=" + this.search
        }
        else{
          url = "https://webmmi.iut-tlse3.fr/~pecatte/librairies/public/20/livres";
        }
        console.log(url);
        fetch(url)
            .then(response => response.json())
            .then(json => {
              this.list = [];
              for (let i = 0; i < json.length; i++) {
                this.list.push(new Livre(json[i].id, json[i].titre, json[i].qtestock, json[i].prix));
              }
            })
            .catch(error => {
              console.error(error);
            });
      },
      deleteItem(id) {
        let myHeaders = new Headers();
        myHeaders.append("content-type", "application/json");
        const fetchOptions = {
          method: "DELETE",
          headers: myHeaders,
        };
        fetch("https://webmmi.iut-tlse3.fr/~pecatte/librairies/public/20/livres/" + id, fetchOptions)
            .then(response => response.json())
            .then(json => {
              this.getLivres();
            })
            .catch(error => {
              console.error(error);
            });
      },
      addQ(livre) {
        console.log(livre);
        let myHeaders = new Headers();
        myHeaders.append("content-type", "application/json");
        let leBody = JSON.stringify( {id: livre.id, titre: livre.titre, qtestock: livre.qtestock+1, prix: livre.prix});
        console.log(leBody)
        const fetchOptions = {
          method: "PUT",
          headers: myHeaders,
          body: leBody};
        fetch("https://webmmi.iut-tlse3.fr/~pecatte/librairies/public/20/livres", fetchOptions)
            .then(response => response.json())
            .then(json => {
              this.getLivres();
            })
            .catch(error => {
              console.error(error);
            });
      },
      removeQ(livre) {
        if(livre.qtestock === 1)this.deleteItem(livre.id);
        let myHeaders = new Headers();
        myHeaders.append("content-type", "application/json");
        let leBody = JSON.stringify({ id: livre.id, titre: livre.titre, qtestock: livre.qtestock-1, prix: livre.prix});
        console.log(leBody)
        const fetchOptions = {
          method: "PUT",
          headers: myHeaders,
          body: leBody};
        fetch("https://webmmi.iut-tlse3.fr/~pecatte/librairies/public/20/livres", fetchOptions)
            .then(response => response.json())
            .then(json => {
              this.getLivres();
            })
            .catch(error => {
              console.error(error);
            });
      },
      showPopup() {
        this.showingPopup = true;

      },
      hidePopup() {
        this.showingPopup = false;
        this.getLivres();
      },
    },
    mounted() {
      this.getLivres();
    },
  }
</script>

<template>
    <popupForm v-if="showingPopup" @close="hidePopup"/>
  <div class="list" v-if="!showingPopup">
    <div class="head-container">
      <button @click="showPopup">Ajouter un livre</button>
      <input type="search" v-model="search" @change="getLivres" placeholder="Rechercher">
    </div>
    <table >
      <thead>
        <tr>
          <th>Couverture</th>
          <th>Titre</th>
          <th>Quantité en stock</th>
          <th>Prix</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <book
            v-for="livre of list"
            :key="livre.id"
            :livre="livre"
            @delete="deleteItem"
            @addQ="addQ"
            @removeQ="removeQ"
        />
      </tbody>
    </table>
  </div>
</template>

<style scoped>

  button{
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 4px;
    padding: 8px 16px;
    cursor: pointer;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    border-spacing: 0;
    margin-bottom: 20px;
  }

  /* Alternating row color */
  tbody tr:nth-child(even) {
    background-color: #f2f2f2;
  }

  /* Table header style */
  thead th {
    font-weight: bold;
    text-align: center;
    background-color: #f2f2f2;
    border: 1px solid #ddd;
    padding: 10px;
  }

  input{
    width: 20%;
    padding: 12px 20px;
    margin: 8px 0;
    box-sizing: border-box;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  .head-container{
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

</style>