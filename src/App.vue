<template>
  <div :class="[{ flexStart: step ===1 }, 'wrapper']">
    <transition name="slide">
      <img src="./assets/logo.svg" class="logo" v-if="step === 1">
    </transition>
    <transition name="fade">
      <HeroImage v-if="step === 0"/>
    </transition>
    <Claim v-if="step === 0"/>
    <SearchInput v-model="searchValue" @input="handleInput" :dark="step === 1" />
    <div class="results" v-if="results && !loading && step === 1">
      <Item v-for="item in results" :item="item" :key="item.data[0].nasa_id" @click.native="handleModalOpen(item)"/>
    </div>
    <div class="loader" v-if="step=== 1 && loading"><div></div><div></div><div></div><div></div></div>
    <Modal v-if="modalOpen" :item="modalItem" @closeModal="modalOpen = false"/>
  </div>
</template>

<script>
import axios from 'axios';

import debounce from 'lodash.debounce';

import Claim from '@/components/Claim.vue';

import SearchInput from '@/components/SearchInput.vue';

import HeroImage from '@/components/HeroImage.vue';

import Item from '@/components/Item.vue';

import Modal from '@/components/Modal.vue';

const API = 'https://images-api.nasa.gov/search';
export default {
  /* eslint max-len: ["error", { "code": 800 }] */
  /* eslint-disable */
  name: 'App',
  components: {
    Claim,
    SearchInput,
    HeroImage,
    Item,
    Modal,
  },
  

  data() {
    return {
      modalOpen: false,
      modalItem: null,
      loading:false,
      step: 0,
      searchValue: '',
      results: [],
    };
  },
  methods: {
    handleModalOpen(item) {
      this.modalOpen = true;
      this.modalItem = item;
    },
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
    background-image: url('./assets/mainimage.jpg') !important;
    background-size: cover;
    background-repeat: no-repeat;
    background-position: 25% 5%;
    background-attachment: fixed;
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity .3s ease;
  }

  .fade-enter, .fade-leave-to{
      opacity: 0;
  }

  .slide-enter-active, .slide-leave-active {
    transition: margin-top .3s ;
  }

  .slide-enter, .slide-leave-to{
      margin-top: -50px;
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
    top:30px;
    
  }

  .results{
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 20px;
    margin-top:50px;

    @media (min-width:768px) {
      grid-template-columns: 1fr 1fr 1fr;
    }

  }
  .results:hover{
    cursor:pointer;
  }

  .loader {
  margin-top:100px;
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;

    @media (min-width: 768px) {
      width: 120px;
      height: 120px;
      
    }
  }
  .loader div {
  box-sizing: border-box;
  display: block;
  position: absolute;
  width: 64px;
  height: 64px;
  margin: 8px;
  border: 8px solid rgb(250, 240, 220);
  border-radius: 50%;
  animation: loading 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  border-color: rgb(250, 240, 220) transparent transparent transparent;

  @media (min-width: 768px) {
      width: 120px;
      height: 120px;
      
    }

}
.loader div:nth-child(1) {
  animation-delay: -0.45s;
}
.loader div:nth-child(2) {
  animation-delay: -0.3s;
}
.loader div:nth-child(3) {
  animation-delay: -0.15s;
}
@keyframes loading {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
