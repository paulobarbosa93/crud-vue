<template>
  <div class="cervejarias">
    <a class="fixo button is-large is-danger is-loading" v-show="isLoading">Loading</a>
    <div class="container">  
      <h1 class="title">{{title}}</h1>
        <div class="columns">
          <div class="column is-5">
            <p class="control has-addons">
              <input class="input is-expanded" type="text" placeholder="Procure pelo nome" v-model="search">
              <a class="button is-info" @click="searchBreweries">Search</a>
            </p>
          </div>
          <div class="column is-5">

          </div>
        </div>
  <div class="columns">
    <div class="column is-12">
      <table class="table is-narrow is-bordered">
        <thead>
          <th>Nome</th>
          <th>Cidade</th>
          <th>Telefone</th>
          <th>Mais</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="brewery in breweries">
          <td>{{brewery.name}}</td>
          <td>{{brewery.city}}</td>
          <td>{{brewery.phone}}</td>
          <td class="is-icon">
            <a href="#">
              <i class="fa fa-map-marker"></i>
            </a>
            <a href="#">
              <i class="fa fa-plus-circle"></i>
            </a>
          </td>
          <td class="is-icon">

            <a href="#">
              <i class="fa fa-edit"></i>
            </a>
            <a href="#">
              <i class="fa fa-trash"></i>
            </a>
          </td>
        </tr>
      </tbody>
    </table>
    <Pagination :total="total" :page="page" :itens-per-page="itensPerPage" @change-page="onChangePage"></Pagination>
  </div>
 </div>
</div>
  </div>
</template>

<script>
import Pagination from './Pagination.vue'

export default {
  data() {
    return {
      isLoading: false,
      title: 'Vue.js Crud',
      search: '',
      breweries: [],
      page: 1,
      total: 0,
      selected: {},
      itensPerPage: 10
    }
  },
  components: {
    Pagination
  },
  methods: {
    showLoading(){
      this.isLoading = true
    },
    hideLoading(){
      this.isLoading = false
    },
    loadBreweries(){
      let t = this
      t.showLoading()

      let start = (t.page * this.itensPerPage) - t.itensPerPage
      let end = t.page * t.itensPerPage

      t.$http.get(`/breweries?_start=${start}_end=${end}`).then(response => {
        t.breweries = response.data
        t.total = response.data.length
      }, error => {
        console.log(error)
      }).finally(function(){
        t.hideLoading();
      })
    },
    searchBreweries(){

    },
    onChangePage(page){
      this.page = page
      this.loadBreweries()
    }
  },
  created(){
    this.loadBreweries()
  }
}
</script>

<style>
  .fixo{
    float: right;
    margin-right: 10px;
    margin-top:
    0px;
    z-index: 1000;
  }
</style>
