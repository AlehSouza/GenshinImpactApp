<template>
  <div class="container">
    <div class="background-filter">
      <div v-if="name" class="characters-filter">
        <div class="sub-characters-filter">
          <h3>Filtro</h3>
          <select v-on:change="changeFilter($event)">
            <option value="id" selected>Nenhum</option>
            <option value="name">Nome</option>
            <option value="weapon">Arma</option>
            <option value="gender">Gênero</option>
            <option value="vision">Visão</option>
          </select>
        </div>
        <h1>{{ name }}</h1>
      </div>
    </div>
    <div class="container-characters-genshin">
      <div class="character-item"
          v-for="(character, i) in characters"
          v-bind:key="i"
          v-bind:style="{backgroundColor: character.color}
      ">
        <span>
          {{character.name}}
        </span>
        <img @click="redirectToPage(character.id)" :src="character.image" :alt="character.name" srcset="">
        <ModalCharacter :character="character"/>
      </div>
    </div>
  </div>
</template>

<script>
import ModalCharacter from './ModalCharacter.vue'
import { redirectToPage } from '@/utils/Pages'

export default {
  components: {
    ModalCharacter
  },
  data () {
    return {
      characters: [],
      charactersDefault: this.api,
      filterOption: '',
      redirectToPage
    }
  },
  props: {
    api: {
      type: Array,
      required: true
    },
    name: {
      type: String,
      required: true
    }
  },
  mounted () {
    this.api.forEach(Character => {
      this.characters.push(Character)
    })
  },
  methods: {
    changeFilter (e) {
      this.filterOption = e.target.options[e.target.options.selectedIndex].value
      if (this.filterOption === 'id') {
        this.characters.sort((a, b) => a.id.toString().localeCompare(b.id))
        return
      }
      this.characters.sort((a, b) => a[e.target.value].localeCompare(b[e.target.value]))
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../styles/styles.scss';

.container {
  h1 {
    margin: 0px;
    padding: 15px;
    color: var(--white-color);
  }
}
.background-filter{
  background-image: url('../assets/backgrounds/night_sky.webp');
  background-size: cover;
  background-position: center;
}
.characters-filter{
  background-color: rgba(0, 0, 0, 0.5);
  padding: 8px;
  justify-content: center;
  align-items: center;
  display: flex;

  .sub-characters-filter{
    position: absolute;
    left: 10vw;
    flex-wrap: wrap;
    padding: 0px 8px;
    justify-content: center;
    display: flex;
    h3{
      margin: auto 0px;
      padding: 0px;
      padding-right: 15px;
      color: var(--white-color)
    }
    select{
      border: 0px;
      border-radius: 50px;
      width: 180px;
      padding: 8px;
      outline: none;
      cursor: pointer;
      text-align: left;
      align-items: left;
      justify-content: left;
      display: flex;
      options {
        cursor: pointer;
      }
    }
  }
}
.container-characters-genshin{
  width: 93vw;
  padding: 25px 0px;
  margin: auto;
  justify-content: center;
  flex-wrap: wrap;
  display: flex;
}
.container-without-data{
  color: black;
}
.character-item{
  overflow: hidden;
  margin: 15px;
  border-radius: 5px;
  box-shadow: 0 5px 10px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
  flex-direction: column;
  justify-content: center;
  align-items: center;
  display: flex;
  span{
    padding: 12px;
    font-weight: 400;
    color: var(--white-color);
  }
  img{
    width: 200px;
    cursor: pointer;
  }
}
</style>
