<template>
  <div class="container mx-auto p-4">
    <h1 class="text-6xl uppercase">Die Unterfränkischen Weltläden</h1>
    <p class="text-xl">... sind Fachgeschäfte des Fairen Handels<br />
      ... treten ein für umweltschonende und menschewürdige Produktionsweisen<br />
      ... machen auf Ungerechtigkeiten im Welthandel aufmerksam und zeigen, dass ein anderes Wirtschaften möglich ist
    </p>
    <h2 class="text-4xl uppercase mt-8">Finden Sie einen Weltladen in Ihrer Nähe:</h2>

    <search-component></search-component>

    <footer class="mt-8 flex flex-row justify-center space-x-6 text-xl">
      <a  href="#">Impressum</a>
      <a  href="#">Datenschutzerklärung</a>
    </footer>
  </div>
</template>

<script>
import SearchComponent from "./components/SearchComponent";

export default {
  name: 'App',
  components: {
    SearchComponent
  }
}
</script>

<style lang="scss">
.container {
  max-width: 960px !important;
}
</style>
