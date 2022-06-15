<template>
  <div class="text-center font-bold rounded-md p-2 bg-red-600 text-white">
    TOTAL OF COSTS
    <br>

    <div class="total-suma">
      <slot>
      </slot>
    </div>    
  </div>
</template>

<script>
export default {
  name: 'TotalCosts',
  props: {
  },
  data () {
    return {
    }
  },
  watch: {
    lengthOfCosts: function (val) {
      console.log(val);
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    
</style>
