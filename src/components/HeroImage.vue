<template>
  <div class="heroImage" />
</template>

<script>
export default {

};
</script>

<style lang="scss" scoped>

.heroImage{
  position:fixed;
  top:0;
  z-index: -1;
  width: 100%;
  height: 100vh;
  background-image: url('../assets/heroimage.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: 50% 0%;
}
.heroImage::after {
  content: '';
  background: rgba(0,0,0, .3);
  position: absolute;
  top:0;
  left:0;
  z-index: 1;
  width:100%;
  height: 100vh;
}
</style>
