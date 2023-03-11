<template>
  <div class="share-bar">
    <div
      class="country-bar"
      :style="'background-color: red; width: ' + this.usa + '%'"
    >
      USA
    </div>
    <div
      class="country-bar"
      :style="'background-color: green; width: ' + this.can + '%'"
    >
      CAN
    </div>
    <div
      class="country-bar"
      :style="'background-color: blue; width: ' + this.ind + '%'"
    >
      IND
    </div>
  </div>
</template>

<script>
export default {
  name: "CountryShareBar",
  props: {
    ind: Number,
    can: Number,
    usa: Number,
  },
};
</script>

<style>
</style>