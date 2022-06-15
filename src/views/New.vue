<template>
  <div class="about">
  </div>
</template>

<script>
export default {
  name: 'New',
  data () {
    return {
    }
  },
  methods: {
  },
  mounted () {
    const shouldStartNewProject = confirm('Are you sure to delete this project and create new?')

    if (shouldStartNewProject) {
      // console.log('------------------------------------');
      // console.log('clear costs');
      // console.log('------------------------------------');

      localStorage.clear('costs')
    } else {
      // console.log('------------------------------------');
      // console.log('cancel');
      // console.log('------------------------------------');
    }
  }
}
</script>
