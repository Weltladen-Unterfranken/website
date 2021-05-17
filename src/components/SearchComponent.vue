<template>
  <div class="search-container flex flex-col-reverse md:flex-row md:max-h-screen py-4 md:space-x-4">
    <div class="list-container flex-1 flex flex-col mt-4 md:mt-0">
      <input v-model="searchTerm"
             type="text"
             class="mb-2 px-4 py-2 text-gray-800"
             placeholder="Suche nach Stadt oder PLZ"
      >

      <ul class="space-y-2 h-64 md:h-full md:max-h-full overflow-y-scroll">
        <li v-for="r in filteredResults"
            class="bg-white cursor-pointer px-4 py-2 text-gray-800 text-xl"
            @click="openResult(r.id)"
            :class="{'active': r.id === selectedEntryId}"
        >
          <span>{{r.name}}</span>
        </li>
      </ul>
    </div>
    <div class="map-container flex-1">
      <l-map ref="map" :center="map.center" :zoom="map.zoom" :options="{gestureHandling: true}" v-if="!selectedEntry">
        <l-tile-layer :url="map.url" :attribution="map.attribution"></l-tile-layer>
        <l-marker
                v-for="r in filteredResults"
                :key="r.id"
                :icon="r.id === selectedEntry ? map.markerRed: map.markerBlue"
                :lat-lng="[r.lat, r.lon]"
                @click="openResult(r.id)">
          <l-tooltip :content="r.name"></l-tooltip>
        </l-marker>
      </l-map>
      <div class="relative bg-white p-8" v-else>
        <i class="absolute close-icon cursor-pointer" @click="closeResult()">
          <svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
               viewBox="0 0 371.23 371.23" style="enable-background:new 0 0 371.23 371.23;" xml:space="preserve">
                <polygon points="371.23,21.213 350.018,0 185.615,164.402 21.213,0 0,21.213 164.402,185.615 0,350.018 21.213,371.23
	                185.615,206.828 350.018,371.23 371.23,350.018 206.828,185.615 "/>
          </svg>
        </i>
        <div class="text-xl text-gray-800">
          <h2>{{selectedEntry.name}}</h2>
          <br />
          <div class="flex">
            <div v-if="hasLogo(selectedEntry)"
                 class="mr-4"
            >
              <img :src="getLogo(selectedEntry)"
                   class="max-w-200"
              />
            </div>
            <div>
            {{selectedEntry.street}}<br/>
            {{selectedEntry.zip}} {{selectedEntry.city}}<br/>
            <span v-if="selectedEntry.web"><a class="underline" target="_blank" rel="noopener" :href="'//'+selectedEntry.web">{{selectedEntry.web}}</a></span>
            </div>
          </div>
          <div class="flex gap-4 mt-8" v-if="hasPhotos(selectedEntry)">
            <img v-for="photo in getPhotos(selectedEntry)"
                 :src="photo"
                 class="max-h-300"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import list from "../assets/data";
  import L from 'leaflet';
  import { GestureHandling } from 'leaflet-gesture-handling';

  L.Map.addInitHook('addHandler', 'gestureHandling', GestureHandling);

export default {
  name: 'SearchComponent',
  data() {
    return {
      map: {
        url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
        attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
        zoom: 9,
        center: [49.9000, 9.7000],
        markerBlue: L.icon({
          iconUrl: require('../assets/images/marker-icon.png'),
          iconSize: [25, 41]
        }),
        markerRed: L.icon({
          iconUrl: require('../assets/images/marker-icon-red.png'),
          iconSize: [25, 41]
        })
      },
      data: list,
      searchTerm: '',
      selectedEntryId: null
    }
  },
  created() {
    this.$nextTick(() =>  this.fitMap())
  },
  computed: {
    filteredResults() {
      const q = this.searchTerm.toLowerCase();
      const d = q.match(/\d+/);
      const digits = d ? d[0].substr(0,3) : null;
      return this.data.filter((entry) => entry.name.toLowerCase().includes(q)
              || entry.zip.startsWith(digits)
              || entry.city.toLowerCase().includes(q));
    },
    mapBounds() {
      const bounds = this.filteredResults.map((e) => [e.lat, e.lon]);
      return bounds.length ? L.latLngBounds(bounds) : null;
    },
    selectedEntry() {
      if(!this.selectedEntryId){
        return null;
      }
      return this.data.find((e) => e.id === this.selectedEntryId);
    }
  },
  methods: {
    openResult(id) {
      this.selectedEntryId = id;
    },
    closeResult() {
      this.selectedEntryId = null;
    },
    fitMap(){
      if(this.mapBounds){
        this.$refs.map.fitBounds(this.mapBounds)
      }
    },
    hasLogo(entry) {
      return !!entry.logo;
    },
    getLogo(entry) {
      return '/assets/logos/' + entry.logo;
    },
    hasPhotos(entry) {
      return !!entry.photo;
    },
    getPhotos(entry) {
      return entry.photo.split('|').map((src) => '/assets/photos/' + src);
    }
  },
  watch: {
    filteredResults() {
      this.fitMap()
    },
    searchTerm(newVal, oldVal) {
      if(newVal === '' && oldVal !== '') {
        this.fitMap()
      }
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
    max-height: 960px;
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

  .active {
    background-color: rgba(255,255,255,0.5)
  }

  .max-h-300 {
    max-height: 300px;
  }

  .max-w-200 {
    max-width: 200px;
  }

</style>
