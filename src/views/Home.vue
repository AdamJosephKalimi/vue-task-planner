<template>
  <div class="home">
    <p>Home</p>
    <div v-if="projects.length">
      <!-- cycle throughthe projects -->
      <div v-for="project in projects" :key="project.id">
        <!-- saving the value of the projects above to the prop :projects -->
        <SingleProject :project="project"/>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
export default {
  name: 'Home',
  data() {
    return {
      projects: [] 
    }
  },
  components: { SingleProject },
  mounted() {
    fetch('http://localhost:3000/projects')
    //gets the response and parses the json
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  }
}
</script>
