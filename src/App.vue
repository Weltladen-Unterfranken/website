<template>
  <div class="container mx-auto p-4">
    <h1 class="text-6xl uppercase main-heading text-center">Die Unter&shy;fränkischen Weltläden</h1>
    <p class="text-xl">... sind Fachgeschäfte des Fairen Handels<br />
      ... treten ein für umweltschonende und menschewürdige Produktionsweisen<br />
      ... machen auf Ungerechtigkeiten im Welthandel aufmerksam und zeigen, dass ein anderes Wirtschaften möglich ist
    </p>
    <h2 class="text-4xl uppercase mt-8">Finden Sie einen Weltladen in Ihrer Nähe:</h2>

    <search-component></search-component>

    <footer class="mt-8 flex flex-row justify-center space-x-6 text-xl">
      <a  href="https://www.weltladen-aschaffenburg.de/impressum-datenschutz" target="_blank">Impressum & Datenschutz</a>
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
  h1.main-heading {
    letter-spacing: 2.2px;
    font-size: 4.5rem;
  }
.container {
  max-width: 1400px !important;
}
</style>
