<template>
  <div class="wrapper">
    <label for="search">Search</label>
    <input
    id="search"
    name="search"
    v-model="searchValue"
    />
  </div>
</template>

<script>
export default {
  name: 'Search',
  data() {
    return {
      searchValue: '',
    };
  },
  methods: {
    handleInput() {
      console.log(this.searchValue);
    },
  },
};
</script>
<style lang="scss" scoped>
.wrapper
  {
  display: flex;
  flex-direction: column;
  align-items: center;
  width:100%;
  margin:0;
  padding:30px;
  }
.search{
  display: flex;
  flex-direction: column;
  width:250px;
}
input{
  height: 30px;
  border: 0;
  border-bottom: 1px solid black;
}
</style>
