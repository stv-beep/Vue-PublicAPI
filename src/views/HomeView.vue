<template>
<div class="home">
    <div class="main">
      <div>
        <img src="https://raw.githubusercontent.com/sethvm/kimiquotes/main/kimi.gif" class="kimi-img" />
        <div class="detail">
          <h3>Kimi Räikkönen Radio Quotes</h3>
          <p>Team radio and interview quotes by Finnish F1 legend Kimi Räikkönen.</p>
        </div>
      </div>
    </div>

    <!-- <form @submit.prevent="SearchQuotes()" class="search-box">
      <input class="year-input" type="text" placeholder="Enter a year..." v-model="search" />
      <input class="search-button" type="submit" value="SEARCH" />
    </form> -->
    <br>
    <div class="quotes-list">
          <h1>LIST OF KIMI'S QUOTES</h1>
      <!-- <div class="quotes-table">
        <table>
          <tbody>
            <tr v-for="quote in quotes" :key="quote.id">
              <router-link :to="'/quote/' + quote.id" class="quote-link">
                <div class="row">
                  <td>{{quote.quote}}</td>
                </div>
              </router-link>
            </tr>
          </tbody>
        </table>
      </div> -->
    </div>

    <!-- DATA GRID -->

     <DxDataGrid id="quotes-table" class="quotes-table" :data-source="quotes"
            key-expr="id"  :show-row-lines="true"
            :show-column-lines="false"
            :show-borders="true"
            @row-click="getDetail">

        
        <DxPager
            :visible="true"
            :allowed-page-sizes="pageSizes"
            :display-mode="displayMode"
            :show-page-size-selector="showPageSizeSelector"
            :show-info="showInfo"
            :show-navigation-buttons="showNavButtons"
        />
        
        <DxFilterRow :visible="showFilterRow" :apply-filter="currentFilter"/>
        <DxHeaderFilter :visible="showHeaderFilter" />
        <DxSearchPanel :visible="true" :width="300" placeholder="Search..."/>

        <!-- quote -->
        <DxColumn
            :width="1200"
            :calculate-filter-expression="calculateFilterExpression"
            data-field="quote"
            alignment="center"
            caption="Quote"
            
        >
        </DxColumn>

        <!-- year -->
        <DxColumn :width="200"
            data-field="year"
            alignment="center"
            caption="Year"
            @click="getDetail"
        >
        </DxColumn>
    </DxDataGrid>


  </div>
</template>

<script>
import axios from 'axios'
//import { ref } from 'vue';

import {
  DxDataGrid,
  DxColumn,
  DxHeaderFilter,
  DxSearchPanel,
  DxFilterRow,
  //DxScrolling,
  DxPager,
  //DxPaging
} from "devextreme-vue/data-grid";
import 'devextreme/dist/css/dx.light.css';

export default ({
  components: {
    DxDataGrid,
    DxColumn,
    DxHeaderFilter,
    DxSearchPanel,
    DxFilterRow,
    //DxScrolling,
    DxPager,
    //DxPaging
  },
  setup () {
    /* const search = ref("");
    const quotes = ref([]);
    const SearchQuotes = () => {
        fetch(`https://kimiquotes.herokuapp.com/quotes/${search.value}`)
        .then(response => response.json())
        .then(data => {
          if (data.error !== undefined){//no quotes
              console.log(data.error)
              alert(data.error)
              //quotes.value = data.error;
              //console.log(quotes.value)
          } else {
              console.log(data)
              quotes.value = data;
          }
        })
    } */
     /* return {
      quotes: [],
      search,
      quotes,
      SearchQuotes
      } */
  },
  data () {
    return {
      quotes: [],
      displayModes: [{ text: 'Display Mode \'full\'', value: 'full' }, { text: 'Display Mode \'compact\'', value: 'compact' }],
        displayMode: 'full',
        pageSizes: [5, 10, 'all'],
        showPageSizeSelector: true,
        showInfo: true,
        showNavButtons: true,
        showFilterRow: true,
        showHeaderFilter: true
    }
  },
  created() {
           axios.get('https://kimiquotes.herokuapp.com/quotes/')
                .then(response => {
                  this.quotes = response.data;
                  console.log(response.data);
                });
  },
  methods: {
    getDetail(e){
      console.log(e)
      this.$router.push({ name: 'Quote Detail', params: { id: e.key } })
    }
  }
})
</script>

<style lang="scss">
body{
      background: -webkit-linear-gradient(left, #25b7c4, #c1fbff);
      background: linear-gradient(to right, #25b7c4, #c1fbff);
      font-family: 'Franklin Gothic Medium', sans-serif;
    }
.home {
  .main {
    position: relative;
    .kimi-img {
      display: block;
      width: 100%;
      height: 520px;
      object-fit: cover;
      position: relative;
      z-index: 0;
    }
    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;
      h3 {
        color:#FFF;
        margin-bottom: 16px;
        font-family: 'Franklin Gothic Medium', sans-serif;
      }
      p {
        color: #FFF;
      }
    }
  }
  .search-box {
    display: flex;
    justify-content: space-around;
    align-items: center;
    padding: 16px;
  }
  .year-input{
        display: block;
        appearance: none;
        border: none;
        outline: none;
        background: none;
        width: 100%;
        color: #FFF;
        background-color: #5ba2ff;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        margin-right: 10px;
        transition: 0.4s;
        &::placeholder {
          color: #f3f3f3;
        }
        &:focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
  }
  .search-button{
        display: block;
        appearance: none;
        border: 1px solid #000;
        outline: none;
        background: none;
        width: 30%;
        color: #FFF;
        background-color: #000000;
        font-size: 20px;
        padding: 9px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        margin-right: 10px;
        transition: 0.4s;
        cursor:pointer;
        &:active {
          background-color: #000000;
        }
        &:hover{
          transition: 0.4s;
          background-color: #5ba2ff;
        }
  }

 .quotes-list {
    padding: 3px;
  }
  #quotes-table {
    margin: 30px;
    padding: 30px;
    
      overflow-x:auto;
      //margin-top: 0px;
      border: 1px solid rgba(255,255,255,0.3);
  }

  
    h1{
      font-size: 30px;
      color: #fff;
      font-weight: 300;
      text-align: center;
      margin-bottom: 15px;
    }
    table{
      width:100%;
      table-layout: fixed;
    }
    .quotes-table{
      //height:300px;
      overflow-x:auto;
      //margin-top: 0px;
      border: 1px solid rgba(255,255,255,0.3);
    }

    td{
      background-color: transparent !important;
      cursor: pointer;
      padding: 15px;
      text-align: left;
      vertical-align:middle;
      font-weight: 300;
      font-size: 22px;
      color: #152841;
      border-bottom: solid 1px rgba(255,255,255,0.1);
      font-family: "Inter",sans-serif;
    }
    td:hover {
    color: lightslategray;
    background-color: #c1fbff !important;
    } 
    
    .row:hover {
      background-color:#c1fbff;
    }
    /*
    ::-webkit-scrollbar {
        width: 6px;
    } 
    ::-webkit-scrollbar-track {
        -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3); 
    } 
    ::-webkit-scrollbar-thumb {
        -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3); 
    } */
  }
  .dx-toolbar-items-container{
    background-color: transparent !important;
  }
  .dx-toolbar{
    background-color: transparent !important;
  }

  /* taula */
  .dx-gridbase-container {
    background-color: transparent;
  }
  .dx-datagrid {
    border-radius: 10px;
  }
  .dx-editor-cell .dx-texteditor, .dx-editor-cell .dx-texteditor .dx-texteditor-input {
    background: transparent;
  }
</style>