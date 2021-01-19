<template>
 <div>
     <h1 class="notification is-info">Detalhes de vendas</h1>
     <div class="container is-flex">
         <div class="form-warp">
            <div class="control">
                <p>Cliente</p>
                <input type="text" class="input" placeholder="Cliente" v-model="client" disabled>
            </div>
            <div class="control pt-5">
                <p>Total da venda</p>
                <input type="text" class="input" placeholder="Total da venda" v-model="total_sale" disabled>
            </div>
             <div class="control pt-5">
                <p>Data da venda</p>
                <input type="text" class="input" placeholder="Data da venda" v-model="date_sale" disabled>
            </div>
         </div>
         <table class="table ml-6">
             <thead>
                 <tr>
                    <th>Produto</th>
                    <th>Qtd comprada</th>
                    <th>Valor</th>
                    <th>Subtotal</th>
                 </tr>
             </thead>
             <tbody>
                 <tr v-for="item in detail_sale.data" :key="item.id">
                     <td>{{item.description}}</td>
                     <td>{{item.qtd}}</td>
                     <td>{{item.price}}</td>
                     <td>{{item.subtotal}}</td>
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
    async created(){
        try{
            let id_sale = this.$route.params.datas.id_sale;
            console.log(id_sale);
            this.detail_sale = await axios.get(`http://localhost:5000/detail/sale/${id_sale}`);
            console.log(this.detail_sale.data);
        }catch(err){
            console.log(err);
        }
    },
    data(){
        return {
            detail_sale: [],
            client: this.$route.params.datas.client,
            total_sale: this.$route.params.datas.total_sale,
            date_sale: this.$route.params.datas.date_sale
        }

    },
    
    methods:{



    }
}
</script>

<style scoped>

</style>