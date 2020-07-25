<template>
  <div class="hero-stuns">
    <v-text-field
      v-model="teamOneHero.stuns"
      label="Stuns"
    ></v-text-field>
  </div>
</template>



<script>
  // TODO selvitä onko import cachettu. Muuten pitää parentista tuoda data
  import HeroesJson from '@/assets/json/heroes.json';
  
  export default {
    name: 'HeroSlection',
    props: ['heroListId'],
    data: () => ({
      heroes: HeroesJson,
      selectedHero: 0
    }),
    computed: {
      sortHeroesByName() {
        return this.heroes.slice(0).sort((a, b) => (a.localized_name > b.localized_name) ? 1 : -1) 
      }
    },
    methods: {
      selectHero() {
        this.$emit("selectHero", { hero_id: this.selectedHero, hero_list_id: this.heroListId });
      }
      
    }
  }
</script>
