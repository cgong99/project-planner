<template>
  <FilterNav @filterChange="current = $event" :current="current"/>
  <div class='home'>
    <div v-if="projects.length">
      
      <div v-for="project in filterProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>

</template>

<script>
// @ is an alias to /src
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'
export default {
  name: 'Home',
  components: {
    SingleProject,
    FilterNav
  },
  data() {
    return {
      projects: [],
      current: 'all'
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then(res => res.json())
      .then(data => this.projects = data)
      .catch(err => console.log(err.message))
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project)=>{
        return project.id !== id
      })
    },
    handleComplete(id) {
      this.projects.forEach(project => {
        if(project.id === id){
          project.complete = !project.complete
        }
      });
    },
  },
  computed: {
    filterProjects() {
      if (this.current === 'completed') {
        return this.projects.filter(project => project.complete)
      } else if (this.current === "Ongoing") {
        return this.projects.filter(project => !project.complete)
      } else {
        return this.projects
      }
    }
  }
}
</script>
