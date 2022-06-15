<template>
  
  <div class="flex flex-row gap-x-4">
    <div 
      v-for="(member, index) in editedMembers" :key="index"
      class="
        w-9 h-9 flex flex-row justify-center
        rounded-full bg-sky-400 text-white font-medium hover:cursor-pointer"
      @click="deleteMember(index)"
    >
      <div class="self-center">{{ member }}</div>
    </div>

    <input
      ref="member"
      v-if="showInput"
      class="h-10 w-10 rounded-full bg-slate-200 text-center uppercase"
      maxlength="2"
      type="text"
    >

    <!-- <div
      v-if="showAddButton"
      class="h-10 w-10 cursor-pointer rounded-full bg-red-600 text-center"
      :class="{'bg-blue-500':showInput}"
      @click="addMember()"
    >
      <div
        class="text-white text-4xl "
        style="line-height:32px;"
      >
        +
      </div>
    </div> -->
    
    <div
      v-if="showAddButton"
      class="h-10 w-10 cursor-pointer rounded-full  text-center
      flex flex-row justify-center items-center"
      @click="addMember()"
    >
      <!-- :class="{'bg-blue-500':showInput}" -->
      <div
        class="h-6 w-6 text-white text-4xl leading-4"
      >
        +
      </div>
    </div>
    
    <!-- <div
      v-if="showInput"
      class="h-10 w-10 cursor-pointer rounded-full  text-center
      flex flex-row justify-center items-center"
      @click="showInput=false"
    >
      <div
        class="h-6 w-6 text-white text-4xl leading-4"
      >
        x
      </div>
    </div> -->
  </div>  
  
  <div class="flex flex-grow gap-x-4">
    <button
      class="w-16 self-end rounded-lg bg-orange-400 text-white"
      @click="saveConfig()"
    >
      SAVE
    </button>
    
    <button
      class="mt-6 w-16 self-end rounded-lg bg-orange-400 text-white"
      @click="cancelConfig()"
    >
      CANCEL
    </button>
  </div>
      
</template>

<script>
export default {
  name: 'EditMembers',
  props: {
    members: Array,
    maxMembers: Number,
  },
  data () {
    return {
      showInput: false,
      newMember: '',
      showAddButton: true,
      editedMembers: [...this.members]
      
    }
  },
  methods: {
    addMember() {
      
      if (!this.showInput) {
        this.showInput = true
      }
      
      if (this.showInput) {

        this.newMember = this.$refs.member.value.toUpperCase()
        
        if (this.newMember.length === 0) {
          this.showInput = false

          return
        }

        if (this.newMember.length === 2) {
          console.log('VALUE:', this.newMember)

          if (this.editedMembers.indexOf(this.newMember) === -1) {
            this.editedMembers.push(this.newMember)

            if (this.editedMembers.length >= this.maxMembers) {
             this.showAddButton = false
            }

            this.newMember = ''
            this.showInput = false

            } else {
              alert('User already exist!')

              return
          }

        } else {
          return
        }

        

      }
    },
    deleteMember(index) {

      if (index === 0) {
        alert('You can delete yourself!')

        return
      }

      if (!this.showInput) {
        if (confirm('Delete member YES/NO?')) {
          this.editedMembers.splice(index, 1)

          console.log('Members:', this.editedMembers)
        }
      }
    },
    saveConfig() {
      this.$emit('save-config', this.editedMembers)
    },
    cancelConfig() {
      this.$emit('cancel-config', true)
    },

  },
  created () {
    if (this.members.length >= this.maxMembers) {
      this.showAddButton = false
    }
  }
}
</script>

<style scoped>

</style>
