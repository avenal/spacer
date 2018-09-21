<template>

    <input id="search" name="search" :value="value" @input="handleChange" :class="{dark:dark}"/>

</template>

<script>
export default{
  name: 'SearchInput',
  props: {
    value: {
      type: String,
      required: true,
    },
    dark: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    handleChange(e) {
      this.$emit('input', e.target.value);
    },
  },
};
</script>
<style lang="scss" scoped>

input{
  display: flex;
  width:300px;
  flex-direction:column;
width:250px;
color:white;
text-align:center;
font-size:18px;
height:30px;
border:0;
border-bottom:1px solid  white;
background:none;
transition:box-shadow .5s ease-out;
@media (min-width: 1024px){
  font-weight:400;
}
}
input:focus{
  outline:none;
  box-shadow: 0 10px 20px -8px rgba(255,255,255,0.3);
}
.dark{
  color:#1e3d4c;
  border-bottom-color: #1e3d4c;
}
.dark:focus {
  box-shadow: 0 10px 20px -8px rgba(#1e3d4a, .2);
}

</style>
