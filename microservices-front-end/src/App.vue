<script lang="ts">
  import { defineComponent } from 'vue';
  import { RouterLink, RouterView } from 'vue-router'

  export default defineComponent({
    name: "App",
    components: {
      RouterLink,
      RouterView
    }
  })
</script>

<template>
  <nav class="navigation">
    <RouterLink to="/">Cadastro</RouterLink>
    <RouterLink to="/products">Listagem</RouterLink>
  </nav>
  <RouterView />
</template>

<style scoped>

</style>
