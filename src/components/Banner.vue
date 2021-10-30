<template>
  <nav>
    <div class="menu">
      <img alt="Migo" class="home" src="../assets/logo.png" >
      <div class="sheet">
        <img alt="Migo" class="home" src="../assets/inventory.png" >
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  // name: 'Banner'
}
</script>

<style scoped>
nav {
  background: #222;
  padding: 0;
}

.menu{
  width: 100%;
  height: 64px;
}
.home {
  position: absolute;
  width: 190px;
  height: 64px;
  left: 0px;
  top: 0px;
  background: #000000;
  box-sizing: border-box;
}
.sheet {
  position: absolute;
  left: 190px;
  right: 0%;
  top: 0%;
  bottom: 0%;
  height: 64px;
  background: #E31E30;
  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.15);
}
</style>
