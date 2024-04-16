<template>
  <div :style="{ ...colorStyle }" class="card">
    <div class="images">
      <img class="imgPokemon" v-if="pokemon.sprites" :src="pokemon.sprites.front_default" alt="Frente Pokemon" />
    </div>
    <div class="texts">
      <h1>{{ pokemon.name }}</h1>
      <p class="spacing">Habilidades</p>
      <div class="ability spacingSmall" :style="{ ...justifyContentStyle }">
        <template v-if="pokemon.abilities">
          <p v-for="(ability, index) in pokemon.abilities" :key="index">
            {{ `${index + 1}:${ability.ability.name}` }}
          </p>
        </template>
      </div>
      <div class="datas spacing">
        <div class="data">
          <p>Peso: {{ (pokemon.weight / 10).toFixed(1) }}Kg</p>
          <p>Tamanho: {{ (pokemon.height / 10).toFixed(1) }}m</p>
          <p>Xp Base: {{ pokemon.base_experience }}</p>
        </div>
        <img @click="playAudio" class="imgIcon" :src="iconPlay" alt="Botão Play" />
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import types from '../assets/types';
  import play from '../assets/play.png';

  export default {
    name: 'CardVue',
    data() {
      return {
        pokemon: {},
        type: [],
        colorStyle: {},
        justifyContentStyle: {},
        iconPlay: play
      }
    },
    props: {
      pokemonId: String
    },  
    methods: {
      getData: async function () {
        try {
          const data = await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.pokemonId}/`);
          this.pokemon = data.data;
          types.forEach(nowType => {
            this.pokemon.types.forEach(pokemonType => {
              if (pokemonType.type.name === nowType.type) {
                this.type.push(nowType);
              }
            });
          });
          this.colorStyle = this.type.length === 1 ? {
            backgroundImage: `linear-gradient(135deg, ${this.changeColor(this.type[0].color,50)}, ${this.type[0].color}, ${this.changeColor(this.type[0].color,-50)} 75%)`
          } : {
            backgroundImage: `linear-gradient(-45deg, ${this.type[0].color}, ${this.type[1].color} 75%)`
          };
          this.justifyContentStyle = this.pokemon.abilities.length === 1 ? {
            justifyContent: 'center'
          } : {
            justifyContent: 'space-between'
          };
        } catch (error) {
          console.log(error)
        }
      },
      playAudio: function () {
        axios.get(this.pokemon.cries.latest, { responseType: 'blob' })
          .then(response => {
            let url = window.URL.createObjectURL(new Blob([response.data]));
            let audio = new Audio();
            audio.src = url;
            audio.play();
            audio.addEventListener('ended', () => {
              window.URL.revokeObjectURL(url);
            });
          })
          .catch(error => {
            console.error('Erro ao carregar o arquivo de áudio:', error);
          });
      },
      changeColor: function (corHex, change) {
        let r = parseInt(corHex.substr(1, 2), 16);
        let g = parseInt(corHex.substr(3, 2), 16);
        let b = parseInt(corHex.substr(5, 2), 16);
        r = Math.min(255, Math.max(0, r + change));
        g = Math.min(255, Math.max(0, g + change));
        b = Math.min(255, Math.max(0, b + change));
        const newColorHex = "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1);
        return newColorHex;
      }

    },
    created() {
      this.getData()
    }
  }
</script>

<style scoped>
  .spacing {
    margin-top: 1.6rem;
  }

  .spacingSmall {
    margin-top: 0.8rem;
  }

  .card {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 36rem;
    position: relative;
    overflow: hidden;
    border-radius: 4.8rem;
    padding: 1.6rem;
    box-shadow: 0 0 12px rgba(255, 255, 255, 0.2);;
  }

  .images {
    display: flex;
    justify-content: center;
  }

  .imgPokemon {
    width: 100%;
    max-width: 35rem;
  }

  .ability {
    display: flex;
    gap: 1.6rem;
    overflow-x: scroll;
    white-space: nowrap;
  }

  .ability::-webkit-scrollbar {
    height: 10px;
  }

  .ability::-webkit-scrollbar-thumb {
    background: #555;
    border-radius: 5px;
    box-shadow: inset 0 0 5px rgba(0, 0, 0);
  }

  .datas {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
  }

  .data {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 17rem;
    padding-bottom: 0.8rem;
    ;
  }

  .imgIcon {
    width: 64px;
    height: 64px;
    cursor: pointer;
  }

  h1,
  p {
    color: #fff;
    font-family: "Press Start 2P", system-ui;
    font-weight: 400;
    text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000, 1px 1px 0 #000;
    text-align: center;
    text-transform: capitalize;
    padding: 1px;
    max-width: 100%;
  }

  h1 {
    font-size: 3.6rem;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }

  p {
    font-size: 1.6rem;
  }

  @media (max-width: 450px) {
    h1 {
      font-size: 2.4rem;
    }

    p {
      font-size: 1.2rem;
    }
  }
</style>
