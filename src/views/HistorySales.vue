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
                    <th>Detalhes</th>
                 </tr>
             </thead>
             <tbody>
                 <tr v-for="item in datas_search" :key="item.id">
                     <td>{{item.id}}</td>
                     <td>{{item.date_sales}}</td>
                     <td>{{item.name}}</td>
                     <td>{{item.total_venda}}</td>
                     <td>
                         <router-link :to="{name: 'DetailSales', params:{datas: {
                             id_sale: item.id,
                             client: item.name,
                             total_sale : item.total_venda,
                             date_sale: item.date_sales
                             }}}">
                             <button class="button is-info">Detalhe</button>                        
                        </router-link>
                    </td>
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
            date_init_process: "",
            datas_search: []
        }

    },
    methods:{
        processDate(date_init){
            let process = date_init.split(' ');
            let date_arr = process[0].toString().split('/');
            return `${date_arr[2]}-${date_arr[1]}-${date_arr[0]} 00:00:00`;
        },

        listItemsPeriod(){            
            axios.post("http://localhost:5000/search/sales/period", {
                date_init: this.processDate(this.date_init),
                date_final: this.processDate(this.date_final)
            }).then(res=>{
                this.datas_search = res.data;
                this.datas_search.forEach((item) => {
                   let date_arr = item.date_sales.toString().split('-');
                   let year = date_arr[0];
                   let month = date_arr[1]
                   let day = date_arr[2].split('T')[0];
                   item.date_sales = `${day}/${month}/${year}`
                    console.log(item.date_sales);
 
               });
            }).catch(err=>{
                console.log(err);
            });
        }

    }
}
</script>

<style scoped>

</style>