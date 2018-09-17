<template>
  <div class="wrapper">
    <Claim />
    <SearchInput/>


  </div>

</template>

<script>
import axios from 'axios'
import debounce from 'lodash.debounce'
import Claim from '@/components/Claim.vue'
import SearchInput from '@/components/SearchInput.vue'
const API ='https://images-api.nasa.gov';

export default {
  name: 'Search',
  components: { Claim, SearchInput },
  data(){
    return{
      searchValue:'',
      results:[],
    };
  },
  methods: {
    handleInput: debounce(function(){
      axios.get(`${API}/search?q=${this.searchValue}&media_type=image`)
      .then((response)=> {
        this.results = response.data.collection.items;
      })
      .catch((error)=>{
        console.log(error);
      });
    },500),
  },
};
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css?family=Montserrat:400,700');
.wrapper{
  display:flex;
  flex-direction:column;
  align-items:center;
  height:100vh;
  width:100%;
  margin:0;
  padding:30px;
  justify-content: center;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: 80% 0%;
  background-image: url('../assets/heroimage.jpg');

}

</style>
