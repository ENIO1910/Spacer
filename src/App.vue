<template>
  <div class="wrapper">
    <HeroImage />
    <Claim />
    <SearchInput v-model="searchValue" @input="handleInput" />
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
      searchValue: '',
      results: [],
    };
  },
  methods: {
    // eslint-disable-next-line
    handleInput: debounce(function() {
      console.log(this.searchValue);
      axios.get(`${API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
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
  .wrapper{
  display: flex;
  flex-direction: column;
  align-items: center;
  width:100%;
  min-height: 100vh;
  margin:0;
  padding:30px;
  justify-content: center;
  }
</style>
