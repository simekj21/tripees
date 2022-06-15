<template>

  <!-- <div class="flex flex-col  
    rounded-md text-md text-white font-bold bg-slate-600">
    <div
      v-for="(debt, index) in viewOfDebts" :key="index"
      class="flex flex-row justify-around"
    >
      <div class="">{{debt.debtor}} &#8594; {{debt.payer}}</div>
      <div>{{debt.amount}} €</div>
    </div>
  </div> -->

<!-- <div class="flex flex-row gap-4"> -->
  <table class="rounded-md border-collapse 
    text-base text-white font-semibold bg-slate-600">
    <tr
      v-for="(debt, index) in viewOfDebts" :key="index"
      class=""
    >
      <td class="px-2">{{debt.debtor}} &#8594; {{debt.payer}}</td>
      <td >{{debt.amount}} €</td>
      <td class="pr-2 text-right">{{Math.round(debt.amount * euCz)}} czk</td>
    </tr>
  </table>
<!-- </div> -->
</template>

<script>
export default {
  name: 'Debts',
  props: {
    costs: Array,
    lengthOfCosts: Number,
    members: Array,
    euCz: Number,
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

      // let costs = []
      // let this.members = ['JA','JI','DA','OL']

      // this.costs.push({amount: 40,type: "Transport",payer: "JA",debtors: ["DA","JI","OL"]})
      // this.costs.push({amount: 30,type: "Transport",payer: "JA",debtors: ["DA","JI"]})
      // this.costs.push({amount: 20,type: "Transport",payer: "JA",debtors: ["DA","JI"]})
      // this.costs.push({amount: 60,type: "Transport",payer: "DA",debtors: ["JI","OL"]})

      // console.log('%cCOSTS', 'padding: 10px;background-color:blue;color:white;')
      // console.table(this.costs)

      let mrizka = {}
      this.members.forEach( pr => {
        this.members.forEach( pr2 => {
            mrizka[pr + pr2] = 0
        })
      })

      // this.costs[0].debtors.forEach(debtor => {
      //   let share = this.costs[0].amount / this.costs[0].debtors.length 
      //   mrizka[debtor + this.costs[0].payer] += share
      // })

      this.costs.forEach( cost => {
          let share = cost.amount / (cost.debtors.length + 1)

          // console.log('Share:', share)
          
          cost.debtors.forEach(debtor => {
          mrizka[debtor + cost.payer] += share
        })
      })

      this.viewOfDebts = []
        
      // console.log('Mrizka:', mrizka)

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

      // console.log('Final debts:', this.viewOfDebts)

      // ANOTHER method to filter double debtors

      // this.viewOfDebts.forEach ((debt,i) =>  {
      //     this.viewOfDebts.forEach( (debtx,ix) => {
      //         if (debt.payer === debtx.debtor && debt.debtor === debtx.payer) {
      //             // console.log('SHODA',i,ix)
      //             // console.log(debt.amount, '/', debtx.amount)
      //             if (debt.amount > debtx.amount) {
      //                 debt.amount = debt.amount - debtx.amount
      //                 debtx.amount = 0
      //             } else {
      //                 debtx.amount = debtx.amount - debt.amount
      //                 debt.amount = 0 
      //             }
      //             // this.viewOfDebts.splice(ix,1)
      //         }
      //     })
      // })

      // this.viewOfDebts = this.viewOfDebts.filter( debt => {
      //   return debt.amount > 0
      // })

      // this.viewOfDebts.sort((a,b) => a.amount - b.amount)

      // console.log('MRIZKA')
      // console.table(mrizka)
      
      // console.log('%cVIEW', 'padding: 10px;background-color:orange;color:white;')
      // console.table(this.viewOfDebts)

      // let payments = []
      // let participiants = ['JA','JI','DA','OL']
      

      // payments.push({amount: 40,type: "Transport",payer: "JA",participiants: "DA,JI,OL"})
      // payments.push({amount: 30,type: "Transport",payer: "JA",participiants: "DA,JI"})
      // payments.push({amount: 20,type: "Transport",payer: "JA",participiants: "DA,JI"})
      // payments.push({amount: 60,type: "Transport",payer: "DA",participiants: "OL,JI"})

      // let mrizka = {}
      // participiants.forEach( pr => {
      //   participiants.forEach( pr2 => {
      //       mrizka[pr + pr2] = 0
      //   })
      // })

      // payments[0].participiants.split(',').forEach(user => {
      //   let share = payments[0].amount / payments[0].participiants.split(',').length 
      //   mrizka[user + payments[0].payer] += share
      // })

      // payments.forEach( payment => {
      //     payment.participiants.split(',').forEach(user => {
      //     let share = payment.amount / payment.participiants.split(',').length 
      //     mrizka[user + payment.payer] += share
      //   })
      // })

      // for (const key in mrizka) {
      //   console.log(`${key}: ${mrizka[key]}`);
      // }



      // this.debtorDuos = []
      // this.debtorDuos.length = 50
      // this.debtorDuos.fill(0)
      // this.overviewDebtsFinal = []

      // this.listCosts.forEach((cost) => {
      //   this.qtDebtors = cost.payingDebtors.filter(item => item).length
      //   this.amountPerOne = Math.floor(cost.amount / this.qtDebtors)

      //   cost.payingDebtors.forEach((isPaying, index) => {
          
      //     if (isPaying) {
      //       this.debtorIndex = parseInt((index+1).toString() + (cost.payingMember+1).toString())
      //       this.debtorDuos[this.debtorIndex] += this.amountPerOne
            
      //     }
      //   })
      // }) 

      // let copyDebtorDuos = [...this.debtorDuos]
     
      // copyDebtorDuos.forEach((amount, index) => {
      //   let indexStr = index.toString()
      //   let oppoIndex = parseInt(indexStr[1] + indexStr[0])
        
      //   if (copyDebtorDuos[oppoIndex] > 0) {
      //     if (copyDebtorDuos[oppoIndex] > copyDebtorDuos[index]) {
      //       copyDebtorDuos[oppoIndex] = copyDebtorDuos[oppoIndex] - amount
      //       copyDebtorDuos[index] = 0
      //     } else {
      //       copyDebtorDuos[index] = amount - copyDebtorDuos[oppoIndex]
      //       copyDebtorDuos[oppoIndex] = 0
      //     }
      //   }
      // })
      
      // copyDebtorDuos.forEach((amount, index) => {
      //   if (amount > 0) {
      //     let indexDebtor = parseInt(index.toString()[0]) - 1
      //     let indexPayer = parseInt(index.toString()[1]) - 1

      //     this.overviewDebtsFinal.push({
      //       debtor: this.members[indexDebtor],
      //       payer: this.members[indexPayer],
      //       amount: amount,
      //     })

      //     this.overviewDebtsFinal.sort((a, b) => {
      //         return a.amount - b.amount
      //     })
      //   }
      // })
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
