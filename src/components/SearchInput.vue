<template>
  <input
  id="search"
  name="search"
  :value="value"
  @input="handleChange"
  placeholder="Moon"
  />
</template>
<script>
export default {
  name: 'SearchInput',
  props: {
    value: {
      type: String,
      required: true,
    },
  },
  methods: {
    handleChange(e) {
      this.$emit('input', e.target.value);
    },
  },
};
</script>

<style  lang="scss" scoped>

input{
height: 30px;
border: 0;
background:none;
border-bottom: 1px solid rgb(250, 240, 220);
caret-color:rgb(250, 240, 220);
color:rgb(250, 240, 220);
text-align:center;
font-size:20px;
transition: .5s;
margin-top:50px;
display: flex;
flex-direction: column;
width:250px;

    @media(min-width: 1024px) {
            font-weight: 400;
        }
}

input:focus {
    outline: none;
    box-shadow: 0 10px 20px -7px rgba(250, 240, 220, .3);
    transition: box-shadow .3s ease-out;
}

</style>
