<template>
  
  <TableComponent 
    :columns="columnHeader" 
    :rows="rows" 
    :totalRows="totalRows"
    :rowsPerPage="rowsPerPage"
    :dataLoading="dataLoading"
    @updateNextPage="nextPage" 
    @updatePreviousPage="previousPage" 
     />
</template>

<script>
import TableComponent from './components/TableComponent.vue'
const  ALL_PEOPLE_URI = "https://swapi.dev/api/people"

export default {
  name: 'App',
  components: {
   
    TableComponent
  },
  computed: {

    columnHeader() {
      let columns = this.list.length ?  Object.keys(this.list[0]) : [];
      return columns.map(this.snakeToCamel)
    },
    rows() {
      let rows = this.list.map(result => Object.values(result));
      return rows
    }
  },
  methods: {
     nextPage() {
      this.updateResults(this.next)
    },
    previousPage() {
            this.updateResults(this.previous)
    },
     snakeToCamel(str) {
      return str.replace( /([-_]\w)/g, g => g[ 1 ].toUpperCase() );
     },
    async fetchData(url) {
      const response = await fetch(url);
      const movies = await response.json();
      console.log(movies);
      return movies;

    },
    handleChildEvent(message) {
      this.childMessage = message;
    },
    async updateResults(url) {
      this.dataLoading = true
      let list = await this.fetchData(url)
      this.dataLoading = false

      this.list = list.results
      this.totalRows = list.count
      this.next = list.next
      this.previous = list.previous
    }
  },
  async mounted() {
      
      await this.updateResults(ALL_PEOPLE_URI)
      this.rowsPerPage = this.list.length
  },
  data() {
    return {
      list: [],
      next: null,
      dataLoading: false,
      previous: null,
      rowsPerPage: 0,
      totalRows: 0
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
