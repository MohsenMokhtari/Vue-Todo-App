<template>
  <div class="bg">
    <v-container>
      <List/>
    </v-container>
  </div>
</template>

<script>
import List from "../components/List.vue";
export default {
  name: "Home",
  components: {
    List
  }
};
</script>
<style scoped >
.bg {
  background-image: linear-gradient(rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.6)),
    url("../../listbg2.jpg");
  background-size: cover;
  background-position: center;
  height: 100vh;
  background-attachment: fixed;
}
</style>

