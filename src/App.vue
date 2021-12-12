<template>
  <div :class="[{ flexStart: step ===1 },'wrapper']">
    <transition name="slide">
      <img src="./assets/logo.svg" class="logo">
    </transition>
    <transition name="fade">
      <HeroImage v-if="step === 0"/>
    </transition>
    <Claim v-if="step === 0"/>
    <SearchInput v-model="searchValue" @input="handleInput" :dark="step === 1" />
  </div>
</template>

<script>
import axios from 'axios';

import debounce from 'lodash.debounce';

import Claim from '@/components/Claim.vue';

import SearchInput from '@/components/SearchInput.vue';

import HeroImage from '@/components/HeroImage.vue';

const API = 'https://images-api.nasa.gov/search';
export default {
  /* eslint-disable */
  name: 'App',
  components: {
    Claim,
    SearchInput,
    HeroImage,
  },
  

  data() {
    return {
      loading:false,
      step: 0,
      searchValue: '',
      results: [],
    };
  },
  methods: {
    // eslint-disable-next-line
    handleInput: debounce(function() {
      this.loading = true;
      console.log(this.searchValue);
      axios.get(`${API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
          this.loading = false;
          this.step = 1;
        })
        .catch((error) => {
          console.log(error);
        });
    }, 800),
  },
};
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600;800&display=swap');
  * {
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  body{
    font-family: 'Montserrat',sans-serif;
    margin:0;
    padding: 0;
    color:rgb(250, 240, 220);
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity .3s ease;
  }

  .fade-enter, .fade-leave-to
    /* .fade-leave-active below version 2.1.8 */ {
      opacity: 0;
    }

  .wrapper{
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  width:100%;
  min-height: 100vh;
  margin:0;
  padding:30px;
  justify-content: center;

  &.flexStart {
    justify-content: flex-start;
    }
  }
  .logo {
    position: absolute;
    top:40px;
    
  }
</style>
