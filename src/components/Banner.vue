<template>
  <div id="main-banner">
    <h1>Tênis Vue Dimas</h1>
  </div>
</template>

<script>
export default {
  name: "Banner",
};
</script>

<style scoped>
#main-banner {
  background-image: url("/public/img/tenis.jpg");
  background-position: 0;
  background-size: cover;
  height: 450px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

#main-banner h1 {
  color: #fff;
  text-align: center;
  font-size: 60px;
  background-color: #222;
  padding: 20px 40px;
  border-top-right-radius: 50px;
  border-bottom-right-radius: 50px;
}
</style>