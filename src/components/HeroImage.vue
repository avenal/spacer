<template >
  <img class="heroImage"/>

</template>
<script>
  export default{
    name:'HeroImage',
  }
</script>
<style lang="scss" scoped>
.heroImage{
  z-index:-1;
  position:fixed;
  height:100vh;
  width:100%;
  top:0;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: 80% 0%;
  background-image: url('../assets/heroimage.jpg');
}
.heroImage::after{
  content:'';
  background:rgba(0,0,0,.3);
  position:absolute;
  top:0;
  left:0;
  z-index:1;
  width:100%;

}
</style>
