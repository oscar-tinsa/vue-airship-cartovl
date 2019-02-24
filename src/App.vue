<template>
  <div class="as-app as-app--nav-left">
    <header class="as-toolbar">
      <div class="as-toolbar__group">
        <img class="as-toolbar__item as-p--8" src="../src/assets/isotipo-tinsa-analytics.png" alt="CARTO">
        <router-link class="as-toolbar__item" to="/"><i class="as-icon-marker"></i></router-link>
        <router-link class="as-toolbar__item" to="/about"><i class="as-icon-settings"></i> </router-link>
      </div>
    </header>
    <div class="as-content">
      <router-view/>
    </div>
  </div>
</template>

<style scoped>
  .as-toolbar {
    background-color: #333;
  }
  .as-toolbar img{
    max-width: 50px;
  }
  .as-toolbar__item i {
    font-size: 20px;
  }
</style>
