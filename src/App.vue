<template>
  <v-app>
    <v-toolbar
      app
    >
      <v-toolbar-side-icon></v-toolbar-side-icon>
      <v-toolbar-title v-text="title"></v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-content>
      <FilteredContainer/>
    </v-content>
  
  </v-app>
</template>

<script>
import FilteredContainer from './components/FilteredContainer'

export default {
  name: 'App',
  components: {
    FilteredContainer
  },
  data () {
    return {
      title: 'Filtered list'
    }
  }
}
</script>
