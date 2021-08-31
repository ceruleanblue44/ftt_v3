<template>
<div class='container' id='app'>
  <TableSearch v-on:childToParent='txtFromChild'  
              v-on:clearField='clearField'
              :sortReset = 'sortReset'
              :isSorted = 'isSorted'
          />
  <!-- debug: sort={{currentSort}}, dir={{currentSortDir}}, num={{numPages()}} input={{searchQuery}} isSorted={{isSorted}}  -->

  <UserTable :userData = 'userData'           
            :styleObj = 'styleObj'  
            :sort = 'sort' 
            :isSorted='isSorted'
            :currentSortDir = 'currentSortDir'
            :getPageItems = 'getPageItems'
            :paginatedData = 'paginatedData'
            :deleteUser = 'deleteUser'
            :sortClass = 'sortClass'
            v-on:passId = 'idFromChild'
         />
         
  <TablePagination :numPages = 'numPages'            
                  :gotoPage = 'gotoPage' 
                  :prevPage = 'prevPage'
                  :nextPage = 'nextPage'
                  :currentPage = 'currentPage'
                  :styleObj = 'styleObj'
              />
                  </div>
</template>

<script>
'use strict';
import TableSearch from './components/TableSearch.vue'
import UserTable from './components/UserTable.vue'
import TablePagination from './components/TablePagination.vue'

export default {
  name: 'App',
  components: {
    TableSearch,
    UserTable, 
    TablePagination,
  },
  data() {
    return {
      userData: [],
      currentSort: 'rating',
      currentSortDir: 'asc',
      currentPage: 1,
      itemsPerPage: 5,
      searchQuery: '',
      isSorted: false,
      styleObj: {
        color: '#1FB2ED',
        fontWeight: '600'
      }     
    }
  },
  methods: {
    getData() {
      fetch('https://5ebbb8e5f2cfeb001697d05c.mockapi.io/users')
        .then(response => response.json())
        .then(data => { 
          data.forEach(user => {
            user.id = parseInt(user.id);
            });     
          this.userData = data;
        });
    },

    sort(key) {     
      if(key === this.currentSort) {
        this.currentSortDir = this.currentSortDir==='asc'?'desc':'asc';
      } 
      this.currentSort = key;
      this.userData = this.sortedData;  
      this.isSorted = true;  
    },

    sortClass(key) {
      if (this.isSorted) {
          if (this.currentSort === key && this.currentSortDir === 'asc') {
            return 'sort_asc'
          } else if (this.currentSort === key && this.currentSortDir === 'desc') {
            return 'sort_desc'
          } 
      }
      return '';
    },

    sortReset() {
    this.userData = this.userData.sort((a, b) => a.id - b.id);
    this.currentPage = 1;
    this.isSorted = false;
    },

    numPages() {
      return Math.ceil(this.filteredData.length / this.itemsPerPage);
    },

    getPageItems(arr) {
      let start = (this.currentPage - 1) * this.itemsPerPage;
      let end = start + this.itemsPerPage;
      return arr.slice(start, end);
    },

    gotoPage(page) {
      this.currentPage = page;
    },

    prevPage() {
      if(this.currentPage > 1) this.currentPage--;
    },

    nextPage() {
      if (this.currentPage * this.itemsPerPage < this.filteredData.length) this.currentPage++;
    },

    txtFromChild(value) {
      this.searchQuery = value;
      this.currentPage = 1;
    },

    clearField() {
      this.searchQuery = '';
    },

    idFromChild(value) {
      this.userIdx = value;
    },

    deleteUser(index) {
      this.userData = this.userData.slice().filter(user => user.id !== index);
    },
  },

  created() {
    this.getData();
  },

  computed: {
    sortedData() {
      return this.userData.slice().sort((a, b) => {
        let modifier = 1;
        if(this.currentSortDir === 'desc') modifier = -1;
        if(a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
        if(a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
        return 0;  
        }); 
      },

    filteredData() {
      return this.userData.slice().filter(user => {
        const username = user.username.toLowerCase();
        const email = user.email.toLowerCase();
        const query = this.searchQuery.toLowerCase();
        return username.startsWith(query) || email.startsWith(query);
        });
      },

    paginatedData() {
      return this.getPageItems(this.filteredData);
    },
  }
}
</script>


