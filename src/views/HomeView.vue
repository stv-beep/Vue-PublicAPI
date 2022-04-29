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
    <br>
    <div class="quotes-list">
        <h1>LIST OF KIMI'S QUOTES</h1>
    
    <!-- DATA GRID -->
     <DxDataGrid id="quotes-table" class="quotes-table" :data-source="quotes"
            key-expr="id"  :show-row-lines="true"
            :show-column-lines="false"
            :show-borders="true"
            @row-click="getDetail">

        <DxPager
            :visible="true"
            :allowed-page-sizes="pageSizes"
            :show-page-size-selector="showPageSizeSelector"
            :show-navigation-buttons="showNavButtons"
        />
        
        <DxSearchPanel :visible="true" :width="300" placeholder="Search..."/>

        <!-- quote -->
        <DxColumn
            :width="1200"
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

  </div></div>
</template>

<script>
import axios from 'axios';

import {
  DxDataGrid,
  DxColumn,
  DxSearchPanel,
  DxPager
} from "devextreme-vue/data-grid";
import 'devextreme/dist/css/dx.light.css';

export default ({
  components: {
    DxDataGrid,
    DxColumn,
    DxSearchPanel,
    DxPager
  },
  data () {
    return {
      quotes: [],
      pageSizes: [5, 10, 20, 'all'],
      showPageSizeSelector: true,
      showNavButtons: true
    }
  },
  created() {
           axios.get('https://kimiquotes.herokuapp.com/quotes/')
                .then(response => {
                  this.quotes = response.data;
                });
  },
  methods: {
    getDetail(e){
      this.$router.push({ name: 'Quote Detail', params: { id: e.key } })
    }
  }
})
</script>

<style lang="scss">
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

 .quotes-list {
    padding: 3px;
  }
  #quotes-table {
    margin: 30px;
    padding: 30px;
    overflow-x:auto;
    border: 1px solid rgba(255,255,255,0.3);
  }
  
    h1{
      font-size: 30px;
      color: #fff;
      font-weight: 300;
      text-align: center;
      margin-bottom: 15px;
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
}

  .dx-toolbar-items-container{
    background-color: transparent !important;
  }
  .dx-toolbar{
    background-color: transparent !important;
  }

  /* table */
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