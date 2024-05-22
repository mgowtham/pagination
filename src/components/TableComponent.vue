<template>
    <div>
      <table v-if="!dataLoading" class="table">
        <thead>
          <tr>
            <th v-for="(column, index) in columns" :key="index">
              {{ column }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(rowData, rowIndex) in rows" :key="rowIndex">
            <td v-for="(rowDatum, colIndex) in rowData" :key="colIndex">
              {{ rowDatum}}
            </td>
          </tr>
        </tbody>
      </table>
      <div v-else> Data Loading</div>
      <div class="pagination">
        <button @click="prevPage" :disabled="isPrevDisabled">Previous</button>
        <span>Page {{ currentPage }} of {{ totalPages }}</span>
        <button @click="nextPage" :disabled="isNextDisabled">Next</button>
      </div>      
    </div>
  </template>
  
  <script>
  export default {
    name: 'TableComponent',
    computed: {
      totalPages() {
        return Math.ceil(this.totalRows / this.rowsPerPage);
      },
      isPrevDisabled() {
        return this.currentPage === 1 || this.dataLoading
      },
      isNextDisabled() {
        return this.currentPage === this.totalPages || this.dataLoading
      }
    
    },    
    props: {
      columns: {
        type: Array,
        required: true,
        default: () => []
      },
      dataLoading: {
        type: Boolean,
        required: true,
        default: false,
      },
      rows: {
        type: Array,
        required: true,
        default: () => []
      },
      totalRows: {
        type: Number,
        default: 1
      },      
      rowsPerPage: {
        type: Number,
        default: () => 1      
      },


    },
    methods: {
      prevPage() {
        if (this.currentPage > 1) {
          this.currentPage--;
        }
        this.$emit('updatePreviousPage');
      },
      nextPage() {
        if (this.currentPage < this.totalPages) {
          this.currentPage++;
        }
        this.$emit('updateNextPage');

      },

  },    
    data() {
      return {
        currentPage: 1
      };
    },
  }
  </script>
  
  <style scoped>
  .table {
    width: 100%;
    border-collapse: collapse;
  }
  
  .table th, .table td {
    border: 1px solid #ddd;
    padding: 8px;
  }
  
  .table th {
    background-color: #f2f2f2;
  }
  </style>
  