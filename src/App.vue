<template>
<div class="app">
  <div :class="[{flexStart: step===1}, 'wrapper']">
    <transition name="slide" v-if="step === 1">
      <img src="./assets/logo.svg" class="logo">
    </transition>
    <transition name="fade">
    <HeroImage v-if="step===0" />
  </transition>
    <Claim v-if="step===0" />
    <SearchInput v-model="searchValue" @input="handleInput" :dark="step===1" />
    <div class="results" v-if="results && !loading && step===1">
      <Item v-for="item in results" :item="item" :key="item.data[0].nasa_id" @click.native="handleModalOpen(item)"/>
    </div>
  </div>
      <div class="lds-dual-ring" v-if="step === 1 && loading" />
  <Modal v-if="modalOpen" @closeModal="modalOpen=false" :item="modalItem"/>
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

const API = 'https://images-api.nasa.gov';

export default {
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
      searchValue: '',
      results: [],
      loading: false,
      step: 0,
      modalOpen: false,
      modalItem: null,
    };
  },

  methods: {
    handleModalOpen(item){
      this.modalOpen=true;
      this.modalItem = item;
    },
    handleInput: debounce(function () {
      this.loading = true;
      console.log(this.searchValue);
      axios.get(`${API}/search?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
          this.loading = false;
          this.step = 1;
        })
        .catch((error) => {
          console.log(error);
        });
    }, 500),
  },
};
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css?family=Montserrat:400,700');
* {
    box-sizing: border-box;
}
body {
    font-family: 'Montserrat';
    margin: 0;
    padding: 0;
}
.wrapper {
    min-height: 100vh;
    display: flex;
    position: relative;
    flex-direction: column;
    align-items: center;
    height: 100vh;
    width: 100%;
    margin: 0;
    padding: 30px;
    justify-content: center;
&.flexStart{
  justify-content: flex-start;
}
}
.fade-enter-active, .fade-leave-active{
  transition: opacity .3s ease;
}
.fade-enter, .fade-leave-to{
  opacity: 0;
}
.slide-enter-active, .slide-leave-active{
  transition: margin-top .3s ease;
}
.slide-enter, .slide-leave-to{
  margin-top: -30px;
}
.logo{
  position: absolute;
  top:30px;
  left: 30px;
}
.results {
margin-top: 30px;
display: grid;
grid-template-columns: 1fr 1fr;
grid-gap: 20px;
@media(min-width:768px)
{
  width:90%;
  grid-template-columns: 1fr 1fr 1fr;
}
}
.lds-dual-ring {
  display: inline-block;
  width: 64px;
  height: 64px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 46px;
  height: 46px;
  margin: 1px;
  border-radius: 50%;
  border: 5px solid #fff;
  border-color: #fff transparent #fff transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}


</style>
