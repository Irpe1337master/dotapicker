<template>
  <div class="hero-select">
    <v-select
      :items="sortHeroesByName"
      item-text='localized_name'
      item-value='id'
      label="Select a hero"
      v-bind:value="value"
      v-on:change="change($event)">
    ></v-select>
  </div>
</template>



<script>
  import HeroesJson from '@/assets/json/heroes.json';
  
  export default {
    name: 'HeroSelection',
    props: {
      value: Number
    },
    model: {
      prop: 'value',
      event: 'change'
    },
    data: () => ({
      heroes: HeroesJson,
    }),
    computed: {
      sortHeroesByName() {
        return this.heroes.slice(0).sort((a, b) => (a.localized_name > b.localized_name) ? 1 : -1) 
      }
    },
    methods: {
      change( value ) {
        this.$emit("change" , value );
      }
      
    }
  }
</script>
