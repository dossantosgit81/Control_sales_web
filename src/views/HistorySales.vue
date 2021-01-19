<template>
 <div>
     <h1 class="notification is-info">Histórico de vendas</h1>
     <div class="container is-flex">
         <div class="form-warp">
            <div class="control">
                <p>Data inicial:</p>
                <input type="text" class="input" placeholder="Data inicial" v-model="date_init">
            </div>
            <div class="control pt-5">
                <p>Data final</p>
                <input type="text" class="input" placeholder="Data final" v-model="date_final">
            </div>
            <div class="control pt-5">
                <button class="button is-primary" @click="listItemsPeriod()">Pesquisar vendas por periodo</button>
            </div>
         </div>
         <table class="table ml-6">
             <thead>
                 <tr>
                    <th>Código</th>
                    <th>Data da venda</th>
                    <th>Cliente</th>
                    <th>Total da venda</th>
                 </tr>
             </thead>
             <tbody>
                 <tr v-for="item in datas_search" :key="item.id">
                     <td>{{item.id}}</td>
                     <td>{{item.date_sales}}</td>
                     <td>{{item.name}}</td>
                     <td>{{item.total_venda}}</td>
                 </tr>
             </tbody>
         </table>
         <div class="control ml-6 ">
            <button class="button is-primary">Emitir relatório</button>
         </div>
     </div>
 </div>
    
</template>

<script>
import axios from 'axios';

export default {
    data(){
        return {
            date_init: "",
            date_final: "",
            datas_search: []
        }

    },
    methods:{

        listItemsPeriod(){
            axios.post("http://localhost:5000/search/sales/period", {
                date_init: this.date_init,
                date_final: this.date_final
            }).then(res=>{
                this.datas_search = res.data;
            }).catch(err=>{
                console.log(err);
            });
        }

    }
}
</script>

<style scoped>

</style>