<template>
  <div class="home">
      <v-form v-model="valid">
        <v-container>
          
          <v-row>
            <v-col cols="12" md="6">
              <h2>Team 1</h2>
            </v-col>
            <v-col cols="12" md="6" class="text-right">
              <v-btn color="primary" @click="addTeamOneHero">
                Add team 1 hero
              </v-btn>
            </v-col>
          </v-row>
          
          <v-card class="mb-3">
            <v-card-text>
              <v-row v-for="(teamOneHero) in heroesByTeam(1)" :key="teamOneHero.id">

                <v-col cols="12" md="6">
                  <v-select
                      :items="sortHeroesByName"
                      item-text='localized_name'
                      item-value='id'
                      v-model="teamOneHero.hero_id"
                      label="Hero"
                      @change="addSelecteHeroName($event)"
                    ></v-select>
                  
                  
                </v-col>
                  
                <v-col cols="12" md="6">
                  <v-text-field
                    v-model="teamOneHero.stuns"
                    label="Stuns"
                  ></v-text-field>
                </v-col>

              </v-row>
            </v-card-text>
          </v-card>
          
          <v-row>
            <v-col cols="12" md="6">
              <h2>Team 2</h2>
            </v-col>
            <v-col cols="12" md="6" class="text-right">
              <v-btn color="primary" @click="addTeamTwoHero">Add team 2 hero</v-btn>
            </v-col>
          </v-row>
          
          <v-card>
            <v-card-text>
              <v-row v-for="(teamTwoHero) in heroesByTeam(2)" :key="teamTwoHero.id">

                <v-col cols="12" md="6">
                  <v-select
                      :items="sortHeroesByName"
                      item-text='localized_name'
                      item-value='id'
                      v-model="teamTwoHero.hero_id"
                      label="Hero"
                      @change="addSelecteHeroName($event)"
                    ></v-select>
                </v-col>

                <v-col cols="12" md="6">
                  <v-text-field
                    v-model="teamTwoHero.stuns"
                    label="Stuns"
                  ></v-text-field>
                </v-col>

              </v-row>
            </v-card-text>
          </v-card>
          
          
          
          <v-row>
            <v-col cols="12" class="text-center mt-5">
              <v-btn color="success" @click="compareTeams">Compare teams</v-btn>
            </v-col>
          </v-row>
          
        </v-container>
      </v-form>
      <v-container>
        
        <v-card v-for="team in teams" :key="team.id" class="mb-5">
            <v-card-text>
              <h3>Team {{ team.team }} details</h3>
              
              <v-row v-for="hero in heroesByTeam( team.team )" v-bind:key="hero.team_id">
                
                <v-col cols="12" md="4">
                  <h4>Hero</h4>
                  {{ hero.hero_id }} - {{ hero.name }}
                </v-col>
                <v-col cols="12" md="2">
                  <h4>Winrate</h4>
                  {{ hero.winrate }} 
                </v-col>
                <v-col cols="12" md="2">
                  <h4>Stuns</h4>
                  {{ hero.stuns }} 
                </v-col>
                
                <v-col cols="12">
                  <v-row>
                    <v-col cols="12" md="2" v-for="matchup in matchUpsByHero( hero.hero_id )" v-bind:key="matchup.id">
                      <!-- <p><b>{{ matchup.hero_name }} disadvantage:</b> {{ matchup.disadvantage }}</p> -->
                      <p><b>Adv. against {{ matchup.vs_hero_name }}:</b><br> {{ matchup.advantage }}</p>
                    </v-col>
                  </v-row>
                </v-col>
                <v-col cols="12"><hr class="my-3"></v-col>
                
              </v-row>
              <hr class="my-3">
              <v-row>
                <v-col cols="12" md="4">
                  
                </v-col>
                <v-col cols="12" md="2">
                  <h4>Total Winrate</h4>
                  <p>{{ team.winrate }}</p>
                </v-col>
                <v-col cols="12" md="2">
                  <h4>Total Stuns</h4>
                  <p>{{ team.stuns }}</p>
                </v-col>
                <v-col cols="12" md="2">
                  <h4>Total hero advantage</h4>
                  <p>{{ team.hero_advantage }}</p>
                </v-col>
              </v-row>
            </v-card-text>
          </v-card>
  
      </v-container>
  </div>
</template>

<script>
// @ is an alias to /src
  
  
import axios from "axios";
import HeroesJson from '@/assets/json/heroes.json';
//import HeroSelection from '@/components/HeroSelection';
//import matchUpTestJson from '@/assets/json/matchUpTestJson.json';

export default {
  name: 'Home',
  components: {
    
  },

  data: () => ({
      valid: '',
      items: '',
      heroes: HeroesJson,
      heroesId: 2,
      matchups: [],
      teams: [{
        team: 1,
        stuns: 0,
        winrate: 0,
        hero_advantage: 0,
        hero_disadvantage: 0
      },{
        team: 2,
        advantage: 0,
        disadvantage: 0,
        stuns: 0,
        winrate: 0,
        hero_advantage: 0,
        hero_disadvantage: 0
      }],
      teamHeroes: [{
        id: 0,
        hero_id: '',
        stuns: '',
        name: '',
        team: 1,
        winrate: 0,
      },
      {
        id: 1,
        hero_id: '',
        stuns: '',
        name: '',
        team: 2,
        winrate: 0,
      }]
      
    }),
    computed: {
      sortHeroesByName() {
        return this.heroes.slice(0).sort((a, b) => (a.localized_name > b.localized_name) ? 1 : -1) 
      }
    },
    methods: {
      validate() {
        return
      },
      addSelectedHero( hero ) {
        console.log("hero SE "+hero);
      },
      heroesByTeam( team ) {
        return this.teamHeroes.filter( hero => hero.team == team );
      },
      matchUpsByHero( hero ) {
        return this.matchups.filter( matchup => matchup.hero_id == hero );
      },
      addSelecteHeroName( event ) {
        let heroDetails = this.heroes.find( hero => hero.id == event );
        this.teamHeroes.find(teamHero => teamHero.hero_id == event).name = heroDetails.localized_name;
      },
      addTeamOneHero() {
        this.teamHeroes.push({
          id: this.heroesId,
          hero_id: '',
          stuns: '',
          name: '',
          team: 1,
          winrate: 0,
        });
      
        this.heroesId += 1;
      },
      removeTeamOneHero( index ) {
        this.teamHeroes.splice(index, 1);
      },
      addTeamTwoHero() {
        this.teamHeroes.push({
          id: this.heroesId,
          hero_id: '',
          stuns: '',
          name: '',
          team: 2,
          winrate: 0,
        });
        
        this.heroesId += 1;
      },
      removeTeamTwoHero( index ) {
        this.teamHeroes.splice(index, 1);
      },
      compareTeams( event ) {
        event.preventDefault();
        /*
         * TEAM ONE DETAILS
         */
        // Lisätään ensimmäisen tiimin herot ja niiden alustavat tiedot. Data tulee heroes.json filusta
        // Lasketaan stunnit heroes datasta
        
        // Luodaan sql, jolla haetaan jokaisen tiimin sankarin winrate API:sta.
        //  https://api.opendota.com/api/explorer?sql= -> Api osoite
        let queryHeroWinrates = '';        
        queryHeroWinrates += 'select ';
        queryHeroWinrates += 'hero_id, ';
        queryHeroWinrates += 'count(distinct match_id) games, ';
        queryHeroWinrates += 'sum(case when radiant_win = (player_slot < 128) then 1 else 0 end)::float/count(1) winrate ';
        queryHeroWinrates += 'FROM public_matches ';
        queryHeroWinrates += 'JOIN public_player_matches using(match_id) ';
        queryHeroWinrates += 'JOIN heroes on public_player_matches.hero_id = heroes.id ';
        queryHeroWinrates += "WHERE start_time >= extract(epoch from now() - interval '48 hour')::int ";
        queryHeroWinrates += 'AND ( ';
        
        let loopCounter = 0;
        // Pyöräytetään herot läpi ensimmäisessä tiimissä ja lisätään kyselyn osa
        Array.prototype.forEach.call(this.teamHeroes, hero => {  
          
          if(loopCounter == 0) {
             queryHeroWinrates += ' public_player_matches.hero_id = '+hero.hero_id+' ';
          } else {
            queryHeroWinrates += ' OR public_player_matches.hero_id = '+hero.hero_id+' ';
          }       
          
          loopCounter++;
          
        });
        
        queryHeroWinrates += ' ) ';        
        queryHeroWinrates += 'GROUP BY hero_id ';
        queryHeroWinrates += 'ORDER BY games desc LIMIT 500';
        
        let heroWinratePromise = [];
        let heroWinrateData = [];
        
        // Haetaan API:sta winrate heroille
        heroWinratePromise.push(  
          axios.get('https://api.opendota.com/api/explorer?sql='+queryHeroWinrates).then(response => {
            heroWinrateData = response.data;            
          })
        );
        
        // Käsitellään winratet API:sta saadulla datalla
        Promise.all(heroWinratePromise).then(() => {          
          // Herojen overall winrate
          this.teamHeroes.forEach((teamHero, index) => {     
            // Haetaan hero_id:n perusteella winrate API datasta
            let heroWinrate = heroWinrateData.rows.find( heroWinrate => heroWinrate.hero_id == teamHero.hero_id );
            let heroWinrateRounded = Number( heroWinrate.winrate ).toFixed( 2 );
           
            // Heron overall winrate
            this.teamHeroes[ index ]['winrate'] = heroWinrateRounded;
            // Koko tiimin yhteenlaskettu winrate. Heroon on määritetlty mihin tiimiin kuuluu.
            let teamIndex = this.teams.findIndex( team => team.team == teamHero.team );          
            this.teams[ teamIndex ].winrate = Number( this.teams[ teamIndex ].winrate ) + Number( heroWinrateRounded );
          });
        });
        
        // Hero matchup
        // https://api.opendota.com/api/heroes/{hero_id}/matchups        
        let heroMatchupData = [];
        let heroMatchUpPromises = [];
        
        // Haetaan mathcup data tiimi kakkoselle
        Array.prototype.forEach.call(this.teamHeroes, teamHero => {
          if ( teamHero.team == 2 ) {
            heroMatchUpPromises.push(  
              axios.get('https://api.stratz.com/api/v1/Hero/'+teamHero.hero_id+'/matchUp?matchLimit=100&take=300').then(response => {
                // Asetetaan matchup data hero ID: mukaan
                heroMatchupData[ teamHero.hero_id ] = response.data;
              })
            );
          }            
          
        });
        
        // Lasketaan stunnien määrä yhteen. 
        // TODO katso jos luuppeja voisi yhdistää yhden alle
        this.teamHeroes.forEach(( hero ) => {          
          this.teams.find( team => team.team == hero.team ).stuns += Number( hero.stuns )
        });
        
        
        /**
         * HUOM. Laskuissa käytetään sekä advantage tietoa että disadvatage.
         * Jostain syystä esim. matchup advantage tiedoista ei löydy kaikkien sankareiden vertailua vain osa
         */
        Promise.all(heroMatchUpPromises).then(() => {
          
          let advantageData = [];
          let disadvantageData = [];
          let heroAdvantageData = [];
          let heroDisadvantageData = [];
          let team2Index = this.teams.findIndex( team => team.team == 2 );
          
          this.teamHeroes.forEach(( teamTwoHero ) => {
            
            // Vertaillaan kuinka tiimi 2 pärjää ykkös tiimiä vs. Sen takia käydään vain team 2
            if ( teamTwoHero.team == 1 ) {
               return;
            }
                        
            advantageData = heroMatchupData[ teamTwoHero.hero_id ]['advantage'][0]['vs'];
            disadvantageData = heroMatchupData[ teamTwoHero.hero_id ]['disadvantage'][0]['vs'];
            
              this.teamHeroes.forEach(( teamOneHero ) => {
                
                if (teamOneHero.team == 2) {
                    return;
                }
                
                heroAdvantageData = advantageData.find( hero2 => hero2.heroId2 == teamOneHero.hero_id );
                heroDisadvantageData = disadvantageData.find( hero1 => hero1.heroId2 == teamOneHero.hero_id );
                
                // Haetaan Heron nimi ja muut tiedot
                let vsHeroDetails = this.heroes.find( vsHeroDetails => vsHeroDetails.id == teamOneHero.hero_id );
                let heroDetails = this.heroes.find( heroDetails => heroDetails.id == teamTwoHero.hero_id );
                
                let vsHeroDisadvantage = false;
                if ( typeof heroDisadvantageData != 'undefined' ) {
                  vsHeroDisadvantage = heroDisadvantageData['synergy'];
                }
                
                let matchupAdvantage = false;
                if ( typeof heroAdvantageData != 'undefined' ) {
                   matchupAdvantage = heroAdvantageData['synergy'];
                }
                
                this.matchups.push(
                  {
                    hero_id: teamTwoHero.hero_id,
                    hero_name: heroDetails.localized_name,
                    vs_hero_id: teamOneHero.hero_id,
                    vs_hero_name: vsHeroDetails.localized_name,
                    disadvantage: vsHeroDisadvantage,
                    advantage: matchupAdvantage
                  }
                );
                
                this.teams[ team2Index ].hero_advantage = this.teams[ team2Index ].hero_advantage +  matchupAdvantage;
                this.teams[ team2Index ].hero_disadvantage = this.teams[ team2Index ].hero_advantage +  vsHeroDisadvantage;
                
                console.log(this.matchups)
               
              });
            
          }); 
          
        });
      }
    }
}
</script>
