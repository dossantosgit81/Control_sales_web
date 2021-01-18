<template>
    <div class="container">      
        <h2 class="notification is-info">Cadastro de Vendas</h2>
        <hr>       
        <h3>Dados do Cliente</h3>
        <hr>
        <div class="columns">
            <div class="column">
                <input class="input is-medium" type="text" placeholder="CPF do cliente" v-model="cpf">
                <button class="button is-primary mt-5" @click="searchClient()">Buscar cliente</button>
            </div>
            <div class="column">
                 <input class="input is-medium" type="text" placeholder="Nome do cliente pesquisado" disabled v-model="name">
            </div>
        </div>

        <hr>
        <h3>Dados do produto</h3>
        <hr>
        <div class="columns">
            <div class="column">
                <p>Código</p>
                <input type="number" class="input" placeholder="Código" v-model="codigoProduct">
                <button class="button is-primary mt-5" @click="searchProduct()">Pesquisar</button>
            </div>
            <div class="column">
                <p>Produto</p>
                <input type="text" class="input" placeholder="Produto" v-model="description">
            </div>
             <div class="column">
                <p>Qtd</p>
                <input type="number" class="input" placeholder="Quantidade" v-model="qtd">
                <button class="button is-info mt-5" @click="addItemCart()">Adicionar item</button>
            </div>
              <div class="column">
                <p>Preço</p>
                <input type="number" class="input" placeholder="Preço" v-model="price">
            </div>
            
        </div>
        <div class="columns">
            <div class="column">
            <table class="table mb-6">
                <thead>
                    <tr>
                        <th>Código:</th>
                        <th>Produto:</th>
                        <th>Qtd:</th>
                        <th>Preço:</th>
                        <th>Subtotal</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in cart " :key="item.codigo">
                        <td>{{item.codigo}}</td>
                        <td>{{item.produto}}</td>
                        <td>{{item.qtd}}</td>
                        <td>{{item.price}}</td>
                        <td>{{subtotal = item.subtotal}}</td>
                    </tr>
                </tbody>
            </table>
            </div>
            <div class="column">
                <p>Total da venda:</p>
               <input class="input is-medium" type="text" placeholder="Total da venda" disabled v-model="total">
               <button class="button is-primary mt-3" @click="showModal()">Pagar</button>
            </div>
        </div>
    <div :class="{modal: true, 'is-active': showModalPayment}">
        <div class="modal-background"></div>
        <div class="modal-content spacing" style="overflow: hidden;">
            <div class="columns">
                <div class="column p-5">
                    <p class="is-white">Dinheiro</p>
                    <input type="number" class="input" placeholder="Dinheiro" v-model="money">
                    <p class="is-white">Cartão</p>
                    <input type="number" class="input" placeholder="Cartão" v-model="card">
                    <p class="is-white">Cheque</p>
                    <input type="number" class="input" placeholder="Cheque" v-model="check">
                     <p class="is-white">Troco</p>
                    <input type="number" class="input" placeholder="Troco" disabled v-model="changeOfSale">
                     <p class="is-white">Total da venda</p>
                    <input type="number" class="input" placeholder="Total da Venda" disabled v-model="total">
                    <p class="is-white">Observações</p>
                    <textarea class="textarea is-primary" v-model="obs" id="" cols="15" rows="5"></textarea>
                    <button class="button is-primary mt-3" @click="finalizeSale()">Finalizar Compra</button>
                </div>>
            </div>
        </div>
        <button class="modal-close is-large" @click="hideModal()"></button>
    </div>
    </div><!--Div pai-->
</template>

<script>
import axios from 'axios';

export default {
    data(){
        return {
            showModalPayment: false,
            idClient: '',
            cpf: '',
            name: '',

            codigoProduct: '',
            description: '',
            price: '',
            qtd : '',

            subtotal: 0.0,
            total: 0.0,
            cart: [],
            itemCart: {},

            money: 0.0,
            card: 0.0,
            check: 0.0,
            changeOfSale: 0.0,
            obs: '',
        }
    },
    methods:{
        hideModal(){
            this.showModalPayment = false
        },
        showModal(){
            this.showModalPayment = true
        },
        searchClient(){
            axios.post("http://localhost:5000/search", {
                cpf: this.cpf
            }).then(res=>{
              this.name = res.data[0].name;
              this.idClient = res.data[0].id;
            }).catch(err=>{
                console.log(err);
            });
        },
        searchProduct(){
            axios.post("http://localhost:5000/find", {
                id: this.codigoProduct
            }).then(res=>{
              this.codigoProduct = res.data[0].id;
              this.description = res.data[0].description;
              this.price = res.data[0].price;
            }).catch(err=>{
                console.log(err);
            });
        },
        addItemCart(){   
            this.itemCart = {
                codigo: this.codigoProduct,
                produto: this.description,
                qtd: this.qtd,
                price: this.price,
                subtotal: this.qtd * this.price
            }

            let total = 0.0
            this.cart.push(this.itemCart);
            this.cart.forEach(e=>{
                total += e.subtotal
            });
            this.total = total;
         
        },
        finalizeSale(){
            /**
             *  money: 0.0,
            card: 0.0,
            check: 0.0,
            changeOfSale: 0.0
             */
            let totalPaid = parseFloat(this.money) + parseFloat(this.card)+ parseFloat(this.check);
            this.changeOfSale = totalPaid - this.total;

           

            axios.post("http://localhost:5000/sales", {
                client: this.idClient,
                total_sale: this.total,
                obs: this.obs
            }).then(res=>{
                console.log("Inutil "+res);
                axios.get("http://localhost:5000/lastid").then(res=>{
                this.cart.forEach(product=>{
                   let qtd_current, qtd_purchased, qtd_update;
                    console.log(product.codigo);
                    axios.get(`http://localhost:5000/stock/${product.codigo}`).then(res=>{
                       qtd_current = res.data.result[0].quantity_stock;
                       qtd_purchased = this.qtd;
                       qtd_update = parseFloat(qtd_current) - parseFloat(qtd_purchased);
                       axios.put("http://localhost:5000/stock", {
                           id: product.codigo,
                           qtd_new: qtd_update
                       }).then(res=>{
                           console.log(res);
                       }).catch(err=>{
                           console.log(err);
                       })

                    }).catch(err=>{
                        console.log(err);
                    })
                    axios.post("http://localhost:5000/cart", {
                        sale_id : res.data.idSale,
                        product_id: product.codigo,
                        qtd: product.qtd,
                        subtotal: product.subtotal
                    }).then(res=>{
                        console.log(res);
                        this.$router.push({name: 'Home'});
                    }).catch(err=>{
                        console.log(err);
                    });
                    console.log(product);
                })

            }).catch(err=>{
                console.log(err);
            });
            }).catch(err=>{
                console.log(err);
            });
        }
    }
}
</script>

<style scoped>
.spacing{
    padding:50px;
}   
.is-white{
    color:white;
}
</style>