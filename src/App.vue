<template>
  <div id="app">
    <header>
      <h2 class="container">검색</h2>
    </header>
    <div class="container">
      <search-form v-bind:value="query" v-on:@reset="onReset" v-on:@submit="onSubmit"></search-form>
      
      <div class="content">
        <div v-if="submitted">
          <search-result v-bind:data="searchResult" v-bind:value="query"></search-result>
        </div>
        <div v-else>
          <tabs v-bind:data="tabs" v-bind:selected-tab="selectedTab" v-on:@change="onClickTab"></tabs>

          <div v-if="selectedTab === tabs[0]">
            <tab-list v-bind:data="keywords" type="keyword" 
              v-on:@click="onClickKeyword"></tab-list>
          </div>
          <div v-else>
            <tab-list v-bind:data="history" type="history" 
              v-on:@remove="onRemoveHistory" v-on:@click="onClickKeyword"></tab-list>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import HistoryModel from './models/HistoryModel.js'
import KeywordModel from './models/KeywordModel.js'
import SearchModel from './models/SearchModel.js'

import FormComponent from './components/FormComponent.vue'
import ResultComponent from './components/ResultComponent.vue'
import ListComponent from './components/ListComponent.vue'
import TabComponent from './components/TabComponent.vue'

export default {
  name: 'app',
  data () {
    return {
      query:'',
      submitted: false,
      searchResult : [],
      keywords:[],
      history:[],
      selectedTab : '',
      tabs : ['추천 검색어', '최근 검색어']
    }
  },
  components: {
    'search-form' : FormComponent,
    'search-result' : ResultComponent,
    'tab-list' : ListComponent,
    'tabs' : TabComponent
  },
  created() {
    this.selectedTab = this.tabs[0];
    this.fetchKeyword();
    this.fetchHistory();
  },
  methods: {
    onSubmit(value){
      this.query = value;
      this.search();
    },
    // onKeyup() {

    // }
    onReset() {
      this.ResetForm();
    },
    ResetForm() {
      console.log('[App] ResetForm()')
      this.query = '';
      this.submitted = false;
      this.searchResult = [];
    },
    search() {
      SearchModel.list().then(data  => {
        this.submitted = true
        this.searchResult = data
      })
      HistoryModel.add(this.query);
      this.fetchHistory();
    },
    fetchKeyword(){
      KeywordModel.list().then(data => {
        this.keywords = data;
      })
    },
    fetchHistory(){
      HistoryModel.list().then(data => {
        this.history = data;
      })
    },
    onClickKeyword(keyword){
      this.query = keyword;
      this.search();
    },
    onRemoveHistory(keyword){
      HistoryModel.remove(keyword);
      this.fetchHistory();
    },
    onClickTab(tabName) {
      this.selectedTab = tabName;
    }
  }
}
</script>

<style>

</style>
