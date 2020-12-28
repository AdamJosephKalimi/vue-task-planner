<template>
  <div class="home">
    <!-- $event is the data being transmitted with the $emit event in FilterNav.vue -->
    <FilterNav @filterChange=" filterShowing = $event" :filterShowing="filterShowing" />
    <div v-if="projects.length">
      <!-- cycle throughthe projects -->
      <div v-for="project in filteredProjects" :key="project.id">
        <!-- saving the value of the projects above to the prop :projects -->
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  data() {
    return {
      projects: [],
      filterShowing: 'all'
    }
  },
  computed: {
    filteredProjects() {
      if (this.filterShowing === 'completed') {
        return this.projects.filter(project => project.complete)
      }
      if (this.filterShowing === 'ongoing') {
        return this.projects.filter(project => !project.complete)
      }
      return this.projects
    }
  },
  components: { SingleProject, FilterNav },
  mounted() {
    fetch('http://localhost:3000/projects')
    //gets the response and parses the json
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => {
        return project.id !== id
      })
    },
    handleComplete(id) {
      for(let i = 0; i < this.projects.length; i++) {
        if (this.projects[i].id === id) {
          return this.projects[i].complete = !this.projects[i].complete
        }
      }
    }
  }
}
</script>
