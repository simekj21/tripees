<template>
  <table 
    class="w-full bg-green-100 rounded-md border-collapse text-sm text-white font-bold 
      bg-slate-600">
    <tr
      v-for="(cost, index) in costs" :key="index"
      class="table-row"
    >
      <td 
        class="px-2 font-light text-red-600"
      >
        {{ getDay(index) }}
      </td>
      <td class="px-2">{{ cost.payer }}</td>
      <td class="px-2 text-xs font-extralight">
        {{ cost.typeOfCost.slice(0,4) }}
      </td>
      <td class="px-2 ">
        {{ cost.description }} 
        <span class="text-xs font-extralight">
          (
        </span>
        <span
          v-for="(debtor, index) in cost.debtors" :key="index"
          class="text-xs font-extralight"
        >
          {{debtor}}
          <span v-if="index < (cost.debtors.length-1)"> </span>
        </span>
        <span class="text-xs font-extralight">
          )
        </span>
      </td>
      <td 
        class="text-gray-300 font-normal"
      >
        
      </td>
      <!-- <td 
        v-for="(debtor, index) in cost.debtors" :key="index"
        class="text-gray-300 font-normal"
      >
        {{debtor}}
      </td> -->
      <td class="px-2 text-white text-right">
        {{ cost.amount }} 
        <span class="font-extralight text-xs">
          {{ currency.name }}&nbsp;
        </span>
      </td>
      <!-- <td class="px-2 text-white">{{ cost.amount }} €</td> -->
      <td>
        <button
          class="font-bold p-1 text-red-600"
          @click="removeCost(index)"
        >
          X
        </button>
      </td>
    </tr>
  </table>

</template>

<script>
export default {
  name: 'Costs',
  props: {
    costs: Array,
    currency: Object,
  },
  data () {
    return {
      // actualDay: this.costs[0].day,
      dayNames: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
    }
  },
  methods: {
    removeCost(index) {
      this.$emit('update-costs', index)
    },
    getDay(index) {
      let day = this.dayNames[this.costs[index].day].slice(0,3)

      if (index > 0 && this.costs[index].day === this.costs[index-1].day) {
        day = ''
      }

      return day
    },
  },
  mounted () {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
  table {
    width: 100%;
  }
</style>
