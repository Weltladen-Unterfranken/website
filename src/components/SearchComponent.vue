<template>
  <div class="flex flex-col-reverse md:flex-row search-container space-x-4">
    <div class="list-container flex-1 flex flex-col">
      <input v-model="searchTerm"
             type="text"
             class="mb-2 px-4 py-2 text-gray-800"
             placeholder="Suche nach Stadt oder PLZ"
      >
      <ul class="space-y-2 max-h-full overflow-y-scroll">
        <li v-for="r in filteredResults"
            class="bg-white cursor-pointer px-4 py-2 text-gray-800 text-xl"
            @click="selectedEntry = r.id"
        >
          <span>{{r.name}}</span>
        </li>
      </ul>
    </div>
    <div class="map-container flex-1" v-if="!selectedEntry">
      <l-map ref="map" :center="map.center" :zoom="map.zoom" :options="{gestureHandling: true}">
        <l-tile-layer :url="map.url" :attribution="map.attribution"></l-tile-layer>
      </l-map>
    </div>
    <div class="info-container flex-1" v-else>
      <article class="bg-white p-8 relative">
        <i class="close-icon cursor-pointer" @click="selectedEntry=null">
          <svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 371.23 371.23" style="enable-background:new 0 0 371.23 371.23;" xml:space="preserve">
              <polygon points="371.23,21.213 350.018,0 185.615,164.402 21.213,0 0,21.213 164.402,185.615 0,350.018 21.213,371.23 185.615,206.828 350.018,371.23 371.23,350.018 206.828,185.615 "/>
          </svg>
        </i>
      </article>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SearchComponent',
  data() {
    return {
      map: {
        url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
        attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
        zoom: 9,
        center: [49.9000, 9.7000],
      },
      searchTerm: '',
      selectedEntry: null
    }
  },
  computed: {
    filteredResults() {
      const q = this.searchTerm;
      return new Array(20).fill({
        id: 1,
        name: 'Weltladen Alzenau',
        street: 'Friedberger Gässchen 5',
        zip: '63755',
        city: 'Alzenau',
        web: 'www.weltladen-alzenau.de'
      }).filter((entry) => entry.name.includes(q) || entry.zip.includes(q) || entry.city.includes(q));
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .map-container, .info-container {
    min-height: 50vh;
  }
  .search-container {
    width: 100%;
    max-height: 90vh;
  }
  .search-container > div {
  }

  .close-icon {
    position: absolute;
    top: 10px;
    right: 10px;
    width: 32px;
    height: 32px;
    border: 1px solid #333;
    border-radius: 100%;
    padding: 8px;

  }
</style>
