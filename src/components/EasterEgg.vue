<template>
  <div>
    <br/>
    <p>{{ quote }}</p>
    <button @click="refreshQuote()">Nouvelle citation</button>
    <div v-if="loading" style="color: orange;">Chargement en cours...</div>
    <div v-if="success" style="color: green;">Le chargement est terminé.</div>
    <div v-if="error" style="color: red;">{{ error }}</div>
  </div>
</template>

<script lang="ts">
import axios from 'axios';
import {defineComponent} from 'vue';

export default defineComponent({
  name: 'EasterEgg',
  data() {
    return {
      loading: false,
      success: false,
      error: '',
      quote: ''
    }
  },
  methods: {
    refreshQuote() {
      console.log('created');
      this.loading = true;
      axios.get('https://kaamelott.hotentic.com/api/random').then((resp) => {
        this.quote = resp.data.citation.citation;
        this.loading = false;
      });
    }
  },
  beforeCreate() {
    console.log('À ce stade, les événements et le cycle de vie ont été initialisés mais data reste inactif.')
  },
  created() {
    this.refreshQuote();
    console.log('A ce stade, this.refreshQuote() est maintenant réactif et la methode refreshQuote() va mettre a jour this.quote = resp.data.citation.citation')
    console.log(this.$el)
  },
  mounted() {
    console.log('A ce stade, mounted permet d\'avoir un total accès au composant réactif , aux modèles et au DOM')
    console.log(this.$el)
  },
  updated() {
    console.log('A ce stade, updated permet d\'accéder au DOM une fois qu\'une propriété ait été modifiée')
    console.log(this.$el)
  }
})
</script>
