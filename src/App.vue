<template>
  <div 
    class="relative py-2 px-4 flex flex-row gap-x-2 m justify-end 
    font-semibold text-sm text-slate-400 bg-slate-700 "
  >
    
    <h2 
      class="cursor-pointer"
    >
      <label for="file-upload" class="cursor-pointer">
        LOAD
      </label>
      <input 
        class="cursor-pointer" 
        id="file-upload" 
        type="file"
        @change="loadCosts"
      />
    </h2>
    
    <h2 
      class="cursor-pointer"
      @click="openSaveMenu()"
    >
      SAVE
    </h2>
    <div
      v-show="shouldOpenSave"
      class="absolute top-12 flex flex-col"
    >
      <input 
        class="p-1 border text-black" 
        id="file-save" 
        type="text"
        v-model="nameFileSave"
      />
        <!-- @change="saveCostsFile" -->
      <button
        class="mt-1 p-1 w-24 self-end rounded-lg bg-gray-500 text-white"
        @click="saveCostsFile()"
      >
        SAVE
      </button>
    </div>
    
    <h2 
      class="cursor-pointer"
      @click="openImportMenu()"
    >
      IMPORT
    </h2>
    <div
      v-show="shouldOpenImport"
      class="absolute top-12 flex flex-col"
    >
      <textarea 
        class=" w-80 h-60 overflow-auto bg-gray-200 text-black text-sm"
        ref="import"
        v-model="importedCosts"
      >
        <!-- @blur="shouldOpenImport = false" -->
      </textarea>
      <button
        class="mt-1 p-1 w-24 self-end rounded-lg bg-gray-500 text-white"
        @click="importCosts()"
      >
        IMPORT
      </button>
    </div>
    
    <h2 
      class="cursor-pointer"
      @click="exportCosts()"
    >
      EXPORT
    </h2>
    <textarea 
      v-show="shouldOpenExport"
      class="absolute top-12 w-80 h-60 overflow-auto bg-gray-200"
      ref="export"
      @focus="selectToExport()"
      @blur="shouldOpenExport = false"
    >
      {{ costs }}
    </textarea>
    
    <h2 
      class="cursor-pointer"
      @click="clearCosts()"
    >
      CLEAR
    </h2>

    <svg 
      class="cursor-pointer"
      style="width:20px;height:20px" 
      viewBox="0 0 24 24"
      @click="openAdminArea()"
    >
      <path fill="currentColor" d="M12,15.5A3.5,3.5 0 0,1 8.5,12A3.5,3.5 0 0,1 12,8.5A3.5,3.5 0 0,1 15.5,12A3.5,3.5 0 0,1 12,15.5M19.43,12.97C19.47,12.65 19.5,12.33 19.5,12C19.5,11.67 19.47,11.34 19.43,11L21.54,9.37C21.73,9.22 21.78,8.95 21.66,8.73L19.66,5.27C19.54,5.05 19.27,4.96 19.05,5.05L16.56,6.05C16.04,5.66 15.5,5.32 14.87,5.07L14.5,2.42C14.46,2.18 14.25,2 14,2H10C9.75,2 9.54,2.18 9.5,2.42L9.13,5.07C8.5,5.32 7.96,5.66 7.44,6.05L4.95,5.05C4.73,4.96 4.46,5.05 4.34,5.27L2.34,8.73C2.21,8.95 2.27,9.22 2.46,9.37L4.57,11C4.53,11.34 4.5,11.67 4.5,12C4.5,12.33 4.53,12.65 4.57,12.97L2.46,14.63C2.27,14.78 2.21,15.05 2.34,15.27L4.34,18.73C4.46,18.95 4.73,19.03 4.95,18.95L7.44,17.94C7.96,18.34 8.5,18.68 9.13,18.93L9.5,21.58C9.54,21.82 9.75,22 10,22H14C14.25,22 14.46,21.82 14.5,21.58L14.87,18.93C15.5,18.67 16.04,18.34 16.56,17.94L19.05,18.95C19.27,19.03 19.54,18.95 19.66,18.73L21.66,15.27C21.78,15.05 21.73,14.78 21.54,14.63L19.43,12.97Z" />
    </svg>
    
  </div>

  <!-- SECTIONS SECTIONS SECTIONS SECTIONS SECTIONS SECTIONS-->

  <!-- <div class="min-h-screen pt-10 flex flex-col items-center bg-green-100"> -->
  <div class="py-5 px-4 flex flex-col gap-y-4 bg-slate-800">
    
    <img class="self-center" width="100" src="./assets/trepees-logo.png">
    
    <div 
      v-show="!showAdminArea"
      class="py-5 px-4 flex flex-col gap-y-4 bg-slate-800"
    >
      
      <details
        ref="newCost"
        :open="showSection.newCost"
        @toggle="toggleSection('newCost')"
      >
        <!-- click="showSection.newCost = !showSection.newCost" -->
        <summary class="cursor-pointer text-left text-sm text-gray-400">
            NEW COST
            <hr><br>
        </summary>
        <new-cost
          :members="members"
          :typesOfCost="typesOfCost"
          @add-cost="addCost"
        />
      </details>
      
      <details
        ref="costs"
        :open="showSection.costs" 
        @toggle="toggleSection('costs')"
      >
        <summary class="cursor-pointer text-left text-sm text-gray-400">
            COSTS
            <hr><br>
        </summary>
        <costs
          :costs="costs"
          @update-costs="updateCosts"
        />
      </details>
      
      <details
        ref="reports"
        :open="showSection.reports"
        @change="toggleSection('reports')"
      >
        <!-- @click="toggleSection('reports')" -->
        <summary class="cursor-pointer text-left text-sm text-gray-400">
            REPORTS
            <hr><br>
        </summary>
        <reports
          :euCz="euCz"
          :members="members"
          :costsData="costs"
        />
      </details>
      
      <details
        ref="debts"
        :open="showSection.debts" 
        @toggle="toggleSection('debts')"
      >
        <summary class="cursor-pointer text-left text-sm text-gray-400">
            DEBTS
            <hr><br>
        </summary>
        <debts
          :euCz="euCz"
          :costs="costs"
          :lengthOfCosts="costs.length"
          :members="members"
        />
      </details>
          
      <details
        ref="types" 
        :open="showSection.types" 
        @toggle="toggleSection('types')"
      >
        <summary class="cursor-pointer text-left text-sm text-gray-400">
          TYPES OF COSTS
            <hr><br>
        </summary>
        <types
          :euCz="euCz"
          :costs="costs"
          :members="members"
          :lengthOfCosts="costs.length"
          :typesOfCost="typesOfCost"
          :sumaOfCosts="sumaOfCosts"
        />
      </details>
    
    <details
        ref="suma"
        :open="showSection.suma" 
        @toggle="toggleSection('suma')"
      >
        <summary class="cursor-pointer text-left text-sm text-gray-400">
            SUMA
            <hr><br>
        </summary>
        <svg height="100" width="100" viewBox="0 0 20 20">
          <circle r="10" cx="10" cy="10" fill="white" />
          <circle r="5" cx="10" cy="10" fill="bisque"
                  stroke="tomato"
                  stroke-width="10"
                  stroke-dasharray="calc(35 * 31.42 / 100) 31.42" />
        </svg>
        <br>
        <total-costs>

          {{ sumaOfCosts }} € &#8718;
          {{ Math.round(sumaOfCosts * euCz) }} cz &#8718;
          {{ Math.round((sumaOfCosts / members.length) * euCz) }} cz
        </total-costs>
      </details>
    </div>
    
    <div v-if="showAdminArea">
      <details
          open
        >
          <!-- click="showSection.newCost = !showSection.newCost" -->
          <summary class="cursor-pointer text-left text-sm text-gray-400">
              EDIT MEMBERS
              <hr><br>
          </summary>
          <edit-members
            :members="members"
            :maxMembers="maxMembers"
            @save-config="saveConfig"
            @cancel-config="cancelConfig"
          />
      </details>
    </div>

    <br><br><br><br><br><br>
  </div>

</template>

<!-- <router-view/> -->

<script>
import NewCost from './components/NewCost.vue'
import Costs from './components/Costs.vue'
import Reports from './components/Reports.vue'
import Debts from './components/Debts.vue'
import Types from './components/Types.vue'
import TotalCosts from './components/TotalCosts.vue'
import EditMembers from './components/EditMembers.vue'

var saveData = (function () {
    var a = document.createElement("a");
    document.body.appendChild(a);
    a.style = "display: none";
    return function (data, fileName) {
        var json = JSON.stringify(data),
            blob = new Blob([json], {type: "octet/stream"}),
            url = window.URL.createObjectURL(blob);
        a.href = url;
        a.download = fileName;
        a.click();
        window.URL.revokeObjectURL(url);
    };
}());

// var data = { x: 42, s: "hello, world", d: new Date() }
// var fileName = "my-download.json"

export default {
  components: {
    NewCost,
    Costs,
    Reports,
    Debts,
    Types,
    TotalCosts,
    EditMembers,
  },
  data () {
    return {
      // members: ['JA','OL','DA','JI','MA','DU'],
      euCz: 24.72,
      maxMembers: 5,

      nameFileSave: '',

      showAdminArea: false,
      showSection: {
        newCost: true,
        costs: false,
        reports: false,
        debts: true,
        types: true,
        suma: true,
      },
      showSectionStorage: {},

      members: ['HO','DA','JI','EV'],
      typesOfCost: 
        ['Transport','Restaurant','Food','Pub','Hotel','Attraction','Other'],
      
      cost: {
        payer: '',
        date: new Date(),
        description: '',
        typeOfCost: 'Transport',
        amount: 0,
        currency: 'EU',
        debtors: [],
      },

      // cost: {
      //   payer: '',
      //   description: '',
      //   date: '',
      //   day: '',
      //   typeOfCost: this.typesOfCost[0],
      //   amount: 40,
      //   currency: 'CZK',
      //   debtors: [],
      // },

      costs: [],
      importedCosts: [],
      
      shouldOpenNav: false,
      shouldOpenExport: false,
      shouldOpenImport: false,
      shouldOpenSave: false,
      
      amount: 0,
      amountPerOne: 0,

      lengthOfCosts: 0,
    }
  },
  computed: {
    sumaOfCosts: function () {
      let suma = 0
      
      this.costs.forEach(cost => {
        suma += cost.amount
      })

      return suma
    }

  },
  watch: {
    lengthOfCosts: function () {
      // console.log('-----UPDATE      -------------------');
      // localStorage.setItem('costs', JSON.stringify(this.costs))
    },
  },
  methods: {
    openAdminArea() {
      this.showAdminArea = !this.showAdminArea
    },
    saveConfig(editedMembers) {
      console.log('Save config:', editedMembers)

      this.members = [...editedMembers]
    },
    cancelConfig(shouldCancel) {
      if (shouldCancel) {
        this.showAdminArea = false
      }
    },
    toggleSection(section) {
      this.showSectionStorage[section] = this.$refs[section].open

      localStorage.setItem('showSection', JSON.stringify(this.showSectionStorage))

      // console.log('Toggle section:', this.showSectionStorage)
      // console.log('Toggle section:',section, '>>>', this.$refs[section].open)
    },
    loadCosts(event) {
      console.log('%cLOAD:','background-color:red;')
      console.log(event.target.files)

      var jsonFile = event.target.files[0]
      var fileread = new FileReader()
      
      fileread.onload = function(e) {
        // var content = e.target.result
        var fileToJSON = JSON.parse(e.target.result) 
        console.log('>>>>', fileToJSON)

        // this.costs = []
        this.costs = [...fileToJSON]
        // this.saveCosts()
        console.log('LOADED COSTS:', this.costs)
        localStorage.setItem('costs', JSON.stringify(this.costs))
      }
      
      fileread.readAsText(jsonFile)

      fileread.addEventListener('loadend', () => {
        console.log('%cLOAD ENDED','background-color:orange')
        this.costs = JSON.parse(localStorage.getItem('costs'))
      })

    },
    saveCostsFile() {
      console.log('%cSAVE TO FILE:','background-color:pink;')
      // console.log(evt.target.value)
      console.log('Name of file:', this.nameFileSave)

      this.nameFileSave += '.json'

      saveData(this.costs, this.nameFileSave)

      // var blob = new Blob(["Welcome to Websparrow.org."],
      //     { type: "text/plain;charset=utf-8" })
      // saveAs(blob, "static.txt")
    
    },
    saveCosts() {
      console.log('%cSAVE:','background-color:green;')
      localStorage.setItem('costs', JSON.stringify(this.costs))
    },
    selectToExport() {
      this.$refs.export.select()
    },
    openImportMenu() {
      if (this.shouldOpenImport) {
        this.$refs.import.focus()
      }

      this.shouldOpenImport = !this.shouldOpenImport
    },
    openSaveMenu() {
      // if (this.shouldOpenImport) {
      //   this.$refs.import.focus()
      // }

      console.log('Save menu:')

      this.shouldOpenSave = !this.shouldOpenSave
    },
    importCosts() {
      // console.log('IMPORTED')
      // console.log('Imported:', this.importedCosts)
      this.costs = JSON.parse(this.importedCosts)
      this.saveCosts()

      this.shouldOpenImport = false

    },
    exportCosts() {
      if (this.shouldOpenExport) {
        this.$refs.export.focus()
        this.$refs.export.select()
      }

      this.shouldOpenExport = !this.shouldOpenExport
    },
    clearCosts() {
      if (confirm('Are you sure to clear All?')) {
        localStorage.clear('costs')
        this.costs = []
      }
    },
    addCost(newCost) {
      // console.log('>>>> New cost:', newCost)

      // this.costs.push({...newCost})
      if (newCost.description === '') {
        newCost.description = 'Cost ' + (this.costs.length + 1)
      }

      this.costs.push({...newCost})

      this.saveCosts()

      // localStorage.setItem('Costs', JSON.stringify(this.costs))

      // console.dir(this.costs)


      // const copyNewCost = {...newCost}
      // this.costs.push(copyNewCost)

      // console.log('New cost:', copyNewCost)

      // this.lengthOfCosts = this.costs.length
    },
    updateCosts(index) {
      this.costs.splice(index, 1)

      this.saveCosts()
      
      // localStorage.setItem('Costs', JSON.stringify(this.costs))

      // this.lengthOfCosts = this.costs.length
    }
  },
  created () {
    
    if (localStorage.getItem('costs')) {
      this.costs = JSON.parse(localStorage.getItem('costs'))
    } else {
      // this.costs.push({day:0, date: new Date().toJSON(),amount: 40,typeOfCost: "Transport",description: "Cost 1", payer: "HO",debtors: ["DA","JI","EV"]})
    }

    if (localStorage.getItem('showSection')) {
      this.showSection = JSON.parse(localStorage.getItem('showSection'))
    }

    this.showSectionStorage = {...this.showSection}

    console.log('Copy section:', this.showSectionStorage)

    // this.costs.push({day:0, date: new Date().toJSON(),amount: 40,typeOfCost: "Transport",description: "Train 1", payer: "JA",debtors: ["OL","DA","GE"]})
    
    // this.costs.push({day:0, date: new Date().toJSON,amount: 80,typeOfCost: "Transport",description: "Train 1", payer: "JA",debtors: ["OL","DA","GE"]})
    // this.costs.push({day:0, date: new Date().toJSON,amount: 80,typeOfCost: "Transport",description: "Train 1", payer: "JA",debtors: ["OL","DA","GE"]})
    // this.costs.push({day:1, date: new Date().toJSON,amount: 80,typeOfCost: "Transport",description: "Train 1", payer: "JA",debtors: ["OL","DA","GE"]})
    // this.costs.push({day:1, date: new Date().toJSON,amount: 80,typeOfCost: "Transport",description: "Train 1", payer: "JA",debtors: ["OL","DA","GE"]})
    // this.costs.push({day:1, date: new Date().toJSON,amount: 80,typeOfCost: "Transport",description: "Train 1", payer: "JA",debtors: ["OL","DA","GE"]})

    // if (!localStorage.getItem('Costs')) {
    //   this.costs.push({day:0, date: new Date().toJSON(),amount: 40,typeOfCost: "Transport",description: "Train 1", payer: "JA",debtors: ["OL","DA","GE"]})
    // } else {
    //   this.costs = JSON.parse(localStorage.getItem('Costs'))
    // }

    // console.log('Costs:', this.costs)
    // let testDate = new Date(this.costs[0].date)
    // console.log('Day:', testDate.getDay())

   // const existsCosts = localStorage.getItem('costs')

    // if (!existsCosts) {

    //   this.costs.push(
    //     {
    //       payingMember: 0,
    //       description: 'Train Rome',
    //       type: 'Transport',
    //       amount: 100,
    //       payingDebtors: [true,true,true,true],
    //     },
    //     // {
    //       //   payingMember: 3,
    //     //   description: 'Bus Rome',
    //     //   type: 'Transport',
    //     //   amount: 90,
    //     //   payingDebtors: [true,false,true,true],
    //     // },
    //   )
  
    //   this.costs.forEach((cost) => {
    //     cost.nameMember = this.members[cost.payingMember]
    //   })
    //   this.lengthOfCosts = this.costs.length

    // } else {
    //   this.costs = JSON.parse(localStorage.getItem('costs'))
    //   console.log('------------------------------------');
    //   console.log('costs exists');
    //   console.log(this.costs);
    //   console.log('------------------------------------');

    //   this.lengthOfCosts = this.costs.length
    // }
  },
  beforeDestroy () {
    // localStorage.setItem('costs', this.costs)

    // let payments = []
    // let participiants = ['JA','JI','DA','OL']

    // payments.push({amount: 40,type: "Transport",payer: "JA",participiants: "DA,JI,OL"})
    // payments.push({amount: 30,type: "Transport",payer: "JA",participiants: "DA,JI"})
    // payments.push({amount: 20,type: "Transport",payer: "JA",participiants: "DA,JI"})
    // payments.push({amount: 60,type: "Transport",payer: "DA",participiants: "OL,JI"})

    // mrizka = {}
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

  },
}
</script>

<style scoped>
  body {
    background-color: gray;
  }

  summary {list-style: none}
  summary::-webkit-details-marker {display: none; }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  table {
    width: 100%;
  }

  input[type="file"] {
    display: none;
  }
  /* .custom-file-upload {
      border: 1px solid #ccc;
      display: inline-block;
      padding: 6px 12px;
      cursor: pointer;
  } */
</style>
