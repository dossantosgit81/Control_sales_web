<template>
 <div>
     <h1 class="notification is-info">Total da venda</h1>
     <div class="container is-flex">
         <div class="form-warp">
            <div class="control">
                <p>Data:</p>
                <input type="text" class="input" v-model="date">
            </div>
            <div class="control pt-5">
                <p>Total da venda</p>
                <input type="number" class="input" v-model="total_sale" disabled>
            </div>
              <div class="control pt-5">
                <button class="button is-primary" @click=" searSale()">Pesquisar</button>
            </div>
         </div>
     </div>
 </div>
    
</template>

<script>
import axios from 'axios';

export default {

    data(){
        return {
            date: "",
            total_sale: 0.0,
        }
    },
    
    methods:{

        searSale(){
            axios.post("http://localhost:5000/total/date", {
                date: this.date
            }).then(res=>{
                this.total_sale = res.data.fields[0].sum;
            }).catch(err=>{
                console.log(err);
            })
        }

    }
}
</script>

<style scoped>

</style>