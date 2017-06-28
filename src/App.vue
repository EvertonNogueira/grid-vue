<template>
  <div id="app">
    <table  class="table table-hover">
      <thead>
        <tr>
          <input type="checkbox" checked v-if="allSelected" v-model="allSelected" v-on:click="selectAll(true)">
          <input type="checkbox" v-else  v-model="allSelected" v-on:click="selectAll(true)">
          <td v-for="coluna in listaDeColunas" :key="coluna.idade">
            {{coluna}}
          </td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="linha in listaDeLinhas"  :key="linha.idade">
          <input type="checkbox" checked v-if="linha.checked" v-model="linha.checked" v-on:click="onSelectLine()">
          <input type="checkbox" v-else  v-model="linha.checked" v-on:click="onSelectLine()">
          <td v-for="coluna in listaDeColunas"  :key="coluna.idade">
            {{GetTextByColumn(linha,coluna)}}
          </td>
        </tr>
      </tbody>
    </table>
        <div v-for="(linha,index) in linhasSelecionadas"  :key="linha.idade">
          <input type="hidden" :value="linha.nome" :name="'ListaDeNomes['+index+']'" v-if="linha.checked">
        </div>
        <button v-on:click="onSubmit()" class="btn btn-sm btn-primary">Enviar</button>
  </div>
</template>

<script>
export default {
  props: {
    linhas: {
      type:Array
    },
    colunas: {
      type: Array
    }
  },
  data() {
    return {
      listaDeLinhas:  this.linhas || [],
      listaDeColunas: this.colunas || [],
      allSelected:false,
      linhasSelecionadas:[]
    }
  },
  methods: {
    GetTextByColumn(linha,coluna) {
      return linha[coluna];
    },
    selectAll(selectAll){
      var vm=this;
      if(selectAll){
        vm.listaDeLinhas.forEach(function(item,index){
          item.checked=vm.allSelected;
        });
      }else{
        if(vm.allSelected){
          vm.listaDeLinhas.forEach(function(item,index){
            item.checked=vm.allSelected;
          });
        }
      }
      
    },
    onSelectLine(){
      var linhasMarcadas=0;
      this.listaDeLinhas.forEach(function(item,index){
        if(item.checked){
          linhasMarcadas++;
        }
      });
     
      if(linhasMarcadas==this.linhas.length){
        this.allSelected=true;
        this.selectAll(false);
      }else{
        if(linhasMarcadas==0){
          this.allSelected=false;
          this.selectAll(false);
        }else{
          this.allSelected=false;
        }
      }
    },
    onSubmit(){
      var vm=this;
      vm.linhasSelecionadas=[];
      vm.listaDeLinhas.forEach(function(item,index){
        if(item.checked){
          vm.linhasSelecionadas.push(item);
        }
      });
    }
  }
}
/* <app :linhas="[
    {nome:'nome 1', idade:1, data:'01/01/2017',checked:false},
    {nome:'nome 2', idade:2, data:'02/02/2017',checked:true},
    {nome:'nome 3', idade:3, data:'03/03/2017',checked:false},
    {nome:'nome 4', idade:4, data:'04/04/2017',checked:true}
  ]"
  :colunas="['nome','data']">
    </app>
    */
</script>

<style>
.body{
  padding-left: 10px;
}
</style>
