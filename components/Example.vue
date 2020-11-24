<template>
  <div class="example-component">
    <h1 class="title">{{ title }}</h1>
    <b-button @click="changeTitle()">Change title</b-button>
    <p class="mt-4">My number is {{ myNumber }}</p>
    <b-button @click="increaseNumber()">Increase number</b-button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: 'Old title',
      myNumber: 0,
      myArray: [],
    }
  },
  created() {
    this.myNumber = 5
  },
  methods: {
    changeTitle() {
      this.title = 'New title'
    },
    increaseNumber() {
      this.myNumber++
    },
  },
}
</script>

<style>
.example-component {
  color: blue;
  font-weight: 700;
}
</style>
