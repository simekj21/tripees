<template>
  <table class="rounded-md border-collapse 
    text-base text-white font-semibold bg-slate-600">
    <tr
      v-for="(debt, index) in viewOfDebts" :key="index"
      class=""
    >

      <span class="font-extralight text-xs">{{ this.currency.name }}</span>

      <td class="px-2">{{debt.debtor}} &#8594; {{debt.payer}}</td>
      <td>{{ debt.amount }}<span class="font-extralight text-xs">&nbsp;{{ currency.name }}</span></td>
      <td class="pr-2 text-right">{{Math.round(debt.amount * currency.rate)}} <span class="font-extralight text-xs">cz</span></td>
    </tr>
  </table>
</template>

<script>
export default {
  name: 'Debts',
  props: {
    costs: Array,
    lengthOfCosts: Number,
    members: Array,
    euCz: Number,
    currency: Object,
  },
  data () {
    return {
      viewOfDebts: [],
      
      listCosts: this.costs,
      cost: {
        payingMember: 0,
        nameMember: 'JA',
        description: '',
        type: 'Transport',
        amount: null,
        payingDebtors: [],
      },
      debtorDuos: [],
      overviewDebtsFinal: [],
      qtDebtors: 0,
      qtMembers: 0,
    }
  },
  watch: {
    lengthOfCosts: function () {
      this.updateDebts()
    },
  },
  methods: {
    updateDebts () {
      let mrizka = {}
      this.members.forEach( pr => {
        this.members.forEach( pr2 => {
            mrizka[pr + pr2] = 0
        })
      })

      this.costs.forEach( cost => {
          let share = cost.amount / (cost.debtors.length + 1)

          // console.log('Share:', share)
          
          cost.debtors.forEach(debtor => {
          mrizka[debtor + cost.payer] += share
        })
      })

      this.viewOfDebts = []

      for (const key in mrizka) {
        let oppositeKey = key.slice(2,4) + key.slice(0,2)

        let differ = mrizka[key] - mrizka[oppositeKey]
        
        if (mrizka[oppositeKey] > 0) {
          mrizka[key] = differ > 0 ? differ : 0
          mrizka[oppositeKey] = differ > 0 ? 0: Math.abs(differ)
          
          // if (mrizka[key] > mrizka[oppositeKey]) {
          //   mrizka[key] = mrizka[key] - mrizka[oppositeKey]
          //   mrizka[oppositeKey] = 0
          // } 
          
          // if (mrizka[key] < mrizka[oppositeKey]) {
          //   mrizka[key] = 0
          //   mrizka[oppositeKey] = mrizka[oppositeKey] - mrizka[key]
          // } 
          
          if (mrizka[key] === mrizka[oppositeKey]) {
            mrizka[oppositeKey] = 0
            mrizka[key] = 0
          } 
        }
      }

      for (const key in mrizka) {
        // console.log(`${key}: ${mrizka[key]}`)

        if (mrizka[key] > 0) {
          this.viewOfDebts.push({
            debtor: key.slice(0,2),
            payer: key.slice(2,4),
            amount: Math.round(mrizka[key]),
            amount: Math.round(mrizka[key]*10)/10,
          })
        }
      }

      // console.log('Final debts:', this.viewOfDebts)
      
      this.viewOfDebts.sort((a,b) => b.amount - a.amount)
    },
  },
  created () {
    this.updateDebts()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  table {
    width: 100%;
  }
</style>
