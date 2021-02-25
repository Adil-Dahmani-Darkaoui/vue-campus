<template>
  <div class="div">
    <input
        type="text"
        placeholder="Search..."
        v-model="input"
        v-on:input="search()"
        ref="input"
        style="width: 100%"
    >
  </div>
  <div v-if="search.length > 0">
    <h1>Liste des villes</h1>
    <City v-for="city of search " :key="city.id" :name="city.name" :weather="city.weather"
          :temperature="city.temperature" :updated-at="city.updatedAt"></City>
  </div>

  <div v-else>
    <h1>Liste des villes</h1>
    <City v-for="city of cities " :key="city.id" :name="city.name" :weather="city.weather"
          :temperature="city.temperature" :updated-at="city.updatedAt"></City>
  </div>

</template>

<script lang="ts">
import City from "./City.vue";
import {defineComponent, computed} from 'vue';
import {useStore} from 'vuex';

export default defineComponent({
  name: 'CitiesListStore',
  components: {
    City
  },
  setup() {
    const store = useStore();
    store.dispatch('getCitiesAsync');
    return {
      cities: computed(() => store.state.cities)
    }
  },
  data: () => ({
    input: ''
  }),
  methods: {
    search: function () {
      let allCities: Array<object> = [];
      for (let city of this.cities) {
        if (city.temperature >= this.input) {
          allCities.push(city)
        }
      }
      console.log(allCities)
      return allCities
    }
  }
})
</script>

<style scoped>
h1 {
  margin: 40px 0 0;
}
</style>
