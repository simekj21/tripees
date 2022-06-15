<template>
  <table 
    class="w-full rounded-md border-collapse text-base text-white bg-slate-600"
    
  >
    <tr
      v-for="(expense, index) in expenses" :key="index"
      class="w-full" 
    >
      <td 
        class="relative z-auto pl-2 text-white font-bold text-sm rounded "
      >
        <!-- :class="bgColorTypes[index]" -->
          {{ expense.type }}
        <div 
          class="h-5 absolute top-0 left-0 opacity-50 z-0"
          :class="[procentSizeTypes[getIndexSize(index)],bgColorTypes[index]]"
        >
          <!-- <div 
            class="pl-2 h-2 text-sm bg-red-500 rounded"
          >

          </div> -->
        </div>
      </td>
      <td class="pr-2 text-sm text-gray-200 text-right">
        {{ expense.amount }} €
      </td>
      <td class="pr-2 text-sm text-gray-200 text-right">
        {{ Math.round(expense.amount * euCz) }} cz
      </td>
      <td class="pr-2 text-sm text-gray-200 text-right">
        {{ Math.round((expense.amount / members.length) * euCz) }} cz
      </td>
      <!-- <td class="pr-2 text-sm text-gray-200 text-right">
        {{ expense.amount }} € &#8718;
        {{ Math.round(expense.amount * euCz) }} cz &#8718;
        {{ Math.round((expense.amount / members.length) * euCz) }} cz
      </td> -->
    </tr>
  </table>
    <!-- <div 
      class="w-[100px] h-[30px]"
      :class="getHeightClass"
    >
    </div>

    <div
      v-for="(suma, index) in sumaOfTypes" :key=index
      class="types-line"
    >
      <div :style="'width:' + getProcentStrip(suma) + '%;'" class="types-strip"></div>
      <div>{{ typesOfCost[index]}}</div>
      <div>{{ suma }}</div>
      <div>{{ getProcentStrip(suma, index)}}</div>
    </div> -->
</template>

<script>
export default {
  name: 'Types',
  props: {
    euCz: Number,
    members: Array,
    costs: Array,
    typesOfCost: Array,
    lengthOfCosts: Number,
    sumaOfCosts: Number,
  },
  data () {
    return {
      sumaOfTypes: [],
      expenses: [],
      heightOfDiv: '30px',
      heightClass: 'h-[90px] bg-pink-300',
      actualHeight: 60,
      // widthOfStripe: 0,
      // bgColorTypes: [
      //   'bg-red-900',
      //   'bg-red-800',
      //   'bg-red-700',
      //   'bg-red-600',
      //   'bg-red-500',
      //   'bg-red-400',
      //   'bg-red-300',
      // ],
      bgColorTypes: [
        'bg-red-300',
        'bg-red-400',
        'bg-red-500',
        'bg-red-600',
        'bg-red-700',
        'bg-red-800',
        'bg-red-900',
      ],
      procentSizeTypes: [
        'w-[10%]',
        'w-[20%]',
        'w-[30%]',
        'w-[40%]',
        'w-[50%]',
        'w-[60%]',
        'w-[70%]',
        'w-[80%]',
        'w-[90%]',
        'w-[100%]',
        'w-[110%]',
        'w-[120%]',
      ],
      // procentSizeTypes: [
      //   'w-20',
      //   'w-24',
      //   'w-28',
      //   'w-32',
      //   'w-36',
      //   'w-40',
      //   'w-44',
      //   'w-48',
      //   'w-52',
      //   'w-56',
      // ],
      // procentSizeTypes: [
      //   'w-14',
      //   'w-16',
      //   'w-20',
      //   'w-24',
      //   'w-28',
      //   'w-32',
      //   'w-36',
      //   'w-40',
      //   'w-44',
      //   'w-48',
      //   'w-52',
      //   'w-56',
      // ],
    }
  },
  computed: {
    widthOfStripe: function () {
      return 20
    },
    getHeightClass: function () {
      let computedHeightClass
      computedHeightClass = 'h-[' + this.actualHeight + 'px] bg-pink-300' 
      return computedHeightClass
      // return 'h-[100px] bg-pink-300'
    }
  },
  methods: {
    getIndexSize(index) {
      // let maxValue = this.expenses[this.expenses.length - 1].amount
      let maxValue = this.expenses[0].amount
      
      // console.log('max:', maxValue)

      let procentValue = Math.round((this.expenses[index].amount / maxValue) * 100)
      let indexValue = Math.floor(procentValue / 10) 
      // console.log('procent:', this.expenses[index].amount, '>>>', procentValue,  '>>>', indexValue)

      return indexValue
    },
    updateSumaOfTypes () {
      // this.sumaOfTypes.length = this.typesOfCost.length
      // this.sumaOfTypes.fill(0)

      // this.costs.forEach(cost => {
      //   this.sumaOfTypes[this.typesOfCost.indexOf(cost.typeOfCost)] += cost.amount
      // })

      this.expenses = []

      // this.typesOfCost.forEach(type => {
      //   let filteredCosts = this.costs.filter( cost => cost.typeOfCost === type )
      //   let amount = filteredCosts.reduce((prev.amount,curr.amount) => prev.amount + curr.amount)

      //   console.log('%c-------->','color: red;font-size:20px;')
      //   console.log(type)
      //   console.dir(filteredCosts)

      //   this.expenses.push({
      //     type: type,
      //     amount: amount
      //   })
      // })
  
      this.typesOfCost.forEach(type => {
        this.expenses.push({
          type: type,
          amount: 0,
        })
      })
     
      this.costs.forEach(cost => {
        let typeIndex = this.typesOfCost.indexOf(cost.typeOfCost)

        // this.expenses[typeIndex].type = cost.typeOfCost
        this.expenses[typeIndex].amount += cost.amount
      })
      
      this.expenses.sort((a,b) => b.amount - a.amount)
      this.expenses = [...this.expenses.filter( expense => expense.amount > 0)]

      

      // this.sumaOfTypes.sort()

    },
    getProcentStrip(suma) {
      let procentStrip = 0

      if ((this.sumaOfCosts > 0) && (suma > 0)) {
        procentStrip = Math.floor((suma / this.sumaOfCosts) * 100) - 13
      }

      return procentStrip
    },
  },
  watch: {
    lengthOfCosts: function (val) {
      // console.log('-------------update types----------');
      // console.log('update types', val);
      // console.log('------------------------------------');

      this.updateSumaOfTypes()
      
      // this.sumaOfTypes = []

      // this.sumaOfTypes.sort((a,b) => {
        //   return a-b
      // })

    },
  },
  created () {
    // console.log('--------START COMP TYPES -----------');
    // console.log();
    // console.log('------------------------------------');

    this.updateSumaOfTypes()
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 
</style>
