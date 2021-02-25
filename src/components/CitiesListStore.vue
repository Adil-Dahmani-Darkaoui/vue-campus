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
  <div>
    <h1>Liste des villes</h1>
    <City v-for="city of cities" :key="city.id" :name="city.name" :weather="city.weather"
          :temperature="city.temperature" :updated-at="city.updatedAt"></City>
  </div>
</template>

<script lang="ts">
import City from "./City.vue";
import {defineComponent, computed} from 'vue';
import {useStore} from 'vuex';
// import {store} from "@/store/store";

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
      const store = useStore();
      store.dispatch('getCitiesAsync');
      let arr: Array<object> = [];
      arr.push(computed(() => parseInt(store.state.cities.temperature, 10) >= parseInt(this.input, 10)))
      // console.log(arr)
      return arr
    }
  }
})
</script>

<style scoped>
h1 {
  margin: 40px 0 0;
}
</style>
