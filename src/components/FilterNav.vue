<template>
    <nav class="filter-nav">
        <!-- :class is taking in data from the props, which is checking what filterShowing is equal to in Home.vue -->
        <button @click="updateFilter('all')" :class="{ active: filterShowing === 'all'}">View All</button>
        <button @click="updateFilter('completed')" :class="{ active: filterShowing === 'completed'}">Completed</button>
        <button @click="updateFilter('ongoing')" :class="{ active: filterShowing === 'ongoing'}">Ongoing</button>
    </nav>
</template>

<script>
export default {
  props: [ 'filterShowing' ],
  methods: {
    updateFilter(showing) {
      this.$emit("filterChange", showing);
    }
  }
};
</script>

<style>
.filter-nav button {
  background: none;
  border: none;
  color: #bbb;
  outline: none;
  font-size: 12px;
  text-transform: uppercase;
  margin-right: 10px;
  letter-spacing: 1px;
  font-weight: bold;
  cursor: pointer;
}
.filter-nav button.active {
    color: #555;
}
</style>
