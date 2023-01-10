<template>
     <div class=" flex flex-col gap-y-5">

      <div class="flex flex-row gap-x-4 justify-center">
        <div 
          v-for="(member, index) in members" :key="index"
          class="
            w-9 h-9 flex flex-col justify-center items-center
            rounded-full bg-sky-400 text-white font-medium hover:cursor-pointer"
          :class="member === cost.payer ? 'bg-red-600 text-white' : ''"
          @click="changePayer(member)"
        >
           {{ member }}
        </div>
      </div>  
     
      <div class="flex flex-col gap-y-2 text-sm text-gray-50">
        <input 
          v-model="cost.description"
          class="p-2 self-stretch rounded-lg bg-slate-600"
          type="text" 
          placeholder="For what money spent?"
        />

        <div class="flex flex-row flex-wrap justify-between gap-2">
          <select 
            class="p-2 rounded-lg text-gray-50 bg-slate-600"
            v-model="cost.typeOfCost"
          >
            <option selected 
              v-for="(typeOfCost, index) in typesOfCost" :key="index"
            >
              {{ typeOfCost }}
            </option>
          </select>
          
          <input 
            class="p-2 self-stretch rounded-lg text-right text-gray-50 bg-slate-600" 
            placeholder="0"
            type="text"
            v-model="cost.amount"
          >
          
          <!-- TODO currency -->
          <!-- <select 
            class=" p-2 rounded-lg text-gray-50 bg-slate-600"
            v-model="cost.currency"
          >
            <option 
              v-for="(currency, index) in currencies" :key="index"
            >
              {{ currency }}
            </option>
          </select> -->
          
        </div>  
        
        <div class="mt-2 flex flex-row justify-center gap-x-2">
            <div 
              v-for="(debtor, index) in debtors" :key="index"
              class="w-8 h-8 flex flex-col justify-center items-center 
                rounded-full text-white text-sm font-thin hover:cursor-pointer
              "
              :class="choosedDebtors[index] ? 'bg-red-600' : 'bg-slate-600'"
              @click="chooseDebtor(debtor, index)"
            >
              {{ debtor }}
            </div>
              <!-- :class="choosedDebtors.indexOf(debtor) !== -1 ? 'bg-red-600' : 'bg-slate-600'" -->
              <!-- :class="choosedDebtors.indexOf(debtor) !== -1 ? 'bg-red-600 text-white' : 'bg-red-600 text-white'" -->
        </div>  
        
        <button
          class="mt-1 p-1 w-24 self-end rounded-lg bg-blue-600 text-white"
          @click="addCost()"
        >
          ADD
        </button>
      </div>  

    </div>
</template>

<script>
export default {
  name: 'NewCost',
  props: {
    members: Array,
    typesOfCost: Array,

  },
  data () {
    return {
      debtors: [],
      choosedDebtors: [],

      cost: {
        payer: '',
        description: '',
        date: '',
        day: '',
        typeOfCost: this.typesOfCost[0],
        amount: 40,
        currency: 'CZK',
        debtors: [],
      },
      
      currencies: ['CZK', 'EUR'],
      dayNames: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
    }
  },
  watch: {
    members: function (val) {
      // console.log('New members:', val)

      this.debtors = [...this.members]
      this.debtors.splice(this.debtors.indexOf(this.cost.payer), 1)
    },
  },
  methods: {
    changePayer(member) {
      this.cost.payer = member

      this.debtors = [...this.members]
      this.debtors.splice(this.members.indexOf(member), 1)
    },
    testDebtor(debtor) {
      // console.log('Test debtor:', debtor)
      
      return 'bg-red-600 text-white'
    },
    chooseDebtor(debtor, index) {
      if (debtor !== this.cost.payer) {
        this.choosedDebtors[index] = !this.choosedDebtors[index]
      }

      //  console.log('Paying Debtors:', this.choosedDebtors)
    },
    addCost() {
      if (parseInt(this.cost.amount) <= 0) {
        alert('Insert amount bigger than zero!')

        return
      }
      
      if (!parseInt(this.cost.amount)) {
        alert('Insert number')

        return
      }
      
      // this.cost.description = 'Tr  ' + new Date().toLocaleTimeString()
      // this.cost.typeOfCost = this.cost.typeOfCost.slice(0,4)

      this.cost.debtors = []

      for (let i = 0; i < this.debtors.length; i++) {
        if (this.choosedDebtors[i]) {
          this.cost.debtors.push(this.debtors[i])
        }
      }

      this.cost.amount = parseInt(this.cost.amount)
      this.cost.date = new Date().toJSON()
      this.cost.day = new Date(this.cost.date).getDay()

      // console.log('New cost:', this.cost)
      // console.log('Debtors:', this.cost.debtors)

      this.$emit('add-cost', this.cost)
    },
  },
  created () {
    // console.log('%cNEW COST:', 'background-color:orange;color:white;padding:5px;')
    // console.log('Members:', this.members)

    this.cost.description = ''

    this.cost.amount = 40
    this.cost.payer = this.members[0]

    this.debtors = [...this.members]
    this.debtors.splice(this.debtors.indexOf(this.cost.payer), 1)

    this.choosedDebtors.length = this.members.length
    this.choosedDebtors.fill(true)
  }
}
</script>

<style scoped>

</style>
