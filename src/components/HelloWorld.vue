<template>
  <div class="tudo">

<v-dialog v-model="show_dialog" width="350" >
      <v-card v-if="selected_pokemon" class="rounded-lg" >
        <v-container>
          <v-row>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${selected_pokemon.id}.png`"
                :alt="selected_pokemon.name"
              />
            </v-col>
            <v-col cols="8">
              <h1>{{ get_name(selected_pokemon) }}</h1>
              <v-chip class="mr-2" label v-for="type in selected_pokemon.types" :key="type.slot">
              {{ type.type.name }}
              </v-chip >
              <v-card-text>
                <v-text-field >Altura: {{ selected_pokemon.height * 2.54 }} cm</v-text-field>
              <div class="mt-2">Peso: {{ selected_pokemon.weight *0.45 }} kg</div>
            </v-card-text>
              </v-col>
          </v-row>
        </v-container>
      </v-card>
</v-dialog>

<v-container>
    <v-flex xs12 sm6 >
          <v-text-field
          v-model="search" label="Digite o nome do seu Pokemon" single-line box 
          ></v-text-field>
        </v-flex>
    <v-container>
            {{ search }}
      <v-row>
        <v-col cols="3" 
        v-for="pokemon in filtered_pokemons" 
        :key="pokemon.name">
          <v-card id="card" @click="show_pokemon(get_id(pokemon))" class="text-center">
            <v-container>
              <h1> {{ get_id (pokemon) }}</h1>
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id (pokemon)}.png`"
                :alt="pokemon.name"
              />
              <h2>{{ get_name(pokemon) }}</h2>
            </v-container>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
</v-container>


</div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'HelloWorld',

  data() {
    return {
      pokemons: [],
      search:"",
      show_dialog: false,
      selected_pokemon: null
    }
  },

  mounted() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=150")
    .then((response) => {
      this.pokemons = response.data.results
    })
  },

  methods: {
    get_id(pokemon) {
      return Number(pokemon.url.split("/")[6]);
    },

    get_name(pokemon){
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)
    },
    show_pokemon(id){
      axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
      .then((response) => {
        this.selected_pokemon = response.data;
        this.show_dialog = !this.show_dialog
      });
      
    }
  },

  computed: {
    filtered_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search)
      })
    }
  }
}
</script>

<style>

#card{
  background-color: #fff;
}

.tudo{
  background: linear-gradient(-45deg, #84ffc9, #aab2ff, #eca0ff);
  background-size: 400% 400%;
  
  -webkit-animation: AnimationName 11s ease infinite;
    animation: AnimationName 11s ease infinite;
}

@-webkit-keyframes AnimationName {
    0%{background-position:51% 0%}
    50%{background-position:50% 100%}
    100%{background-position:51% 0%}
}

@keyframes AnimationName {
    0%{background-position:51% 0%}
    50%{background-position:50% 100%}
    100%{background-position:51% 0%}
}
</style>