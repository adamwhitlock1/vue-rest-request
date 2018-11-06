<template>
  <div class="container">
    <app-header></app-header>
    <div class="row">
      <rest-getter></rest-getter>
    </div>
</div>
</template>

<script>
import AppHeader from "./Header";
import RestGetter from "./RestGetter";

export default {
  components: {
    AppHeader,
    RestGetter
  }
};
</script>