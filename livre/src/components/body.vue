<script>
import List from "../components/list.vue";
export default {
  name: "body",
  components: {
    List,
  },
}
</script>

<template>
  <div class="header">
    <img src="./img/logoIsis.png" alt="logoHeader">
    <h1>Librairie ISIS</h1>
  </div>

  <div class="main">
    <List></List>
  </div>

</template>

<style scoped>
  .header{
    /* put the header at the top of the page */
    display: flex;
    left: 0;
    top: 0;
    width: 100%;

    background-color: #283593;
    padding: 30px;
    text-align: center;
  }

  h1{
    color: white;
    font-weight: bold;
    font-size: 5em;
  }

  img{
    width: 15%;
    margin-right: 20%;
  }

  .main{
    padding: 5% 10% 10%;
  }
</style>