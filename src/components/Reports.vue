<template>
  <h1
    class="p-1 mb-2 w-auto rounded-md text-sm text-white font-bold bg-slate-700"
  >
    Items: {{ lengthOfCosts }} 
  </h1>
  
  
  <div
    v-for="(member, index) in members" :key="index"
    v-show="costsData.find(cost => cost.payer === member)"
    class="flex flex-col"
  >
    <h1
      class="p-1 mb-2 w-auto rounded-md text-sm text-white font-bold bg-slate-700"
    >
      {{ member }}
    </h1>
    <table 
      class="w-full mb-4 bg-green-100 rounded-md border-collapse text-sm text-white font-bold 
        bg-slate-600"
    >
        <!-- v-for="(cost, index) in costsData.filter(cost => cost.payer === member)" :key="index" -->
      <tr
        v-for="(cost, index) in filterCostsByMember(member)" :key="index"
        class="table-row"
      >
        <td 
          class="px-2 font-light text-red-600"
        >
          {{ getDay(cost.day) }}
          <!-- {{ getDay(costs.date) }} -->
        </td>
        <td class="px-2">{{ cost.payer }}</td>
        <td class="px-2 text-xs font-extralight">
          {{ cost.typeOfCost.slice(0,4) }}
        </td>
        <td class="px-2">
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
        <td class="px-2 text-white text-right">{{ cost.amount }} €</td>
        
      </tr>
      <tr>
        <td 
          colspan="6"
          class="text-red-400 text-right pr-2 text-lg"
        >
          {{ getSumaByMember(member) }} 
          <span class="font-extralight text-xs">{{ this.currency.name }}&nbsp;</span>&#8718;
          {{ Math.round(getSumaByMember(member) * this.currency.rate) }} 
          <span class="font-extralight text-xs">cz</span>


          <!-- {{ getSuma(costsData.filter(cost => cost.payer === member)) }} € &#8718; 
          {{ Math.round(getSuma(costsData.filter(cost => cost.payer === member)) * euCz) }} cz  -->
        </td>
      </tr>
    </table>
  </div>
  <h1
    class="p-1 mb-2 w-auto rounded-md text-xl text-red-400 text-right font-bold bg-slate-700"
  >
    {{ getSuma(costsData) }} 
    <span class="font-extralight text-xs">{{ this.currency.name }}&nbsp;</span>&#8718;
    {{ Math.round(getSuma(costsData) * this.currency.rate) }}
    <span class="font-extralight text-xs">cz</span>
     
  </h1>
</template>

<script>
export default {
  name: 'Reports',
  props: {
    euCz: Number,
    members: Array,
    costsData: Array,
    currencyRate: Number,
    currency: Object,
  },
  data () {
    return {
      // actualDay: this.costs[0].day,
      dayNames: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
      costsTables: [],
    }
  },
  watch: {
    // whenever question changes, this function will run
    // costsData(newValue, oldValue) {
    //   console.log('CHANGE')
    //   if (newValue.length === oldValue.length) {
    //     console.log('CHANGE')
    //   }
    // }
  },
  computed: {
    lengthOfCosts: function () {
      return this.costsData.length
    },
  },
  methods: {
    filterCostsByMember(member) {
      return this.costsData.filter(cost => cost.payer === member)
    },
    getDay(dayIndex) {
      let day = this.dayNames[dayIndex].slice(0,3)

      return day
    },
    getSuma(costs) {
      let suma = 0
      costs.forEach(cost => suma += cost.amount)

      return suma
    },
    getSumaByMember(member) {
      let suma = 0

      this.filterCostsByMember(member).forEach(cost => suma += cost.amount)
      // this.costsData.forEach(cost => suma += cost.amount)

      return suma
    },
  },
}
</script>