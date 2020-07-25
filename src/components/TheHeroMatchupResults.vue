<template>
<div class="hero-matchup-results">
  <v-container>
    <div v-for="team in teams" :key="team.id" class="mb-5 ">

      <h2>Team {{ team.team }} details</h2>
      <v-row>
        <v-col cols="12" md="4" v-for="hero in heroesByTeam( team.team )" v-bind:key="hero.team_id">
          <v-card
            class="mx-3 my-12"
            max-width="374"
            >
            <v-img
             height="250"
             :src="require(`@/assets/dota.jpg`)"
             ></v-img>

            <v-card-title>{{ hero.hero_id }} - {{ hero.name }}</v-card-title>

            <v-card-text>
              <div>
                <span class="text-uppercase mt-2">WINRATE: </span> <span :class="{'red--text': hero.winrate < 0.5, 'green--text': hero.winrate > 0.5}" >{{ hero.winrate }}</span>
              </div>

              <div>
                <span class="text-uppercase mt-2">STUNS: </span> <span class="red--text">{{ hero.stuns }}</span>
              </div>
            </v-card-text>

            <v-divider class="mx-4"></v-divider>

            <v-card-title>Matchup against team 1</v-card-title>

            <v-card-text>
              <v-list-item v-for="matchup in matchUpsByHero( hero.hero_id )" v-bind:key="matchup.id">
                <v-list-item-content>
                  <v-list-item-title><b>Adv. against {{ matchup.vs_hero_name }}:</b><br> {{ matchup.advantage }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-card-text>

        </v-card>
      </v-col>    
    </v-row>
      
    <v-row>
        <v-col cols="12">
          <v-card
            class="mx-3 my-12"
            >
            <v-card-text>
              
              <v-row>
                <v-col cols="12" md="4">
                  <div class="text-center">
                    <v-card-title class="text-uppercase justify-center">Winrate count</v-card-title>
                    <span class="green--text">{{ team.winrate }}</span>
                  </div>
                </v-col> 
                <v-col cols="12" md="4">
                  <div class="text-center">
                    <v-card-title class="text-uppercase justify-center">Stun count</v-card-title>
                    <span class="red--text">{{ team.stuns }}</span>
                  </div>
                </v-col>
                <v-col cols="12" md="4">
                  <div class="text-center">
                    <v-card-title class="text-uppercase justify-center">Combined hero advantage</v-card-title>
                    <span >{{ team.hero_advantage }}</span>
                  </div>
                </v-col>
              </v-row>             
              
            </v-card-text>
          </v-card>
        </v-col>    
    </v-row>

    </div>
  </v-container>
</div>

</template>

<script>

  export default {
    name: 'HeroMatchupResults',
    props: [ 'teams', 'teamHeroes', 'matchups' ],
    methods: {
      heroesByTeam( team ) {
        return this.teamHeroes.filter( hero => hero.team == team );
      },
      matchUpsByHero( hero ) {
        return this.matchups.filter( matchup => matchup.hero_id == hero );
      },
    }


  };

</script>
