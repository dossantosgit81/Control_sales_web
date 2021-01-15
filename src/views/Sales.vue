<template>
    <div class="container">
    
       
        <h2 class="notification is-info">Cadastro de Vendas</h2>
        <hr>
        

        <h3>Dados do Cliente</h3>
        <hr>
        <div class="columns">
            <div class="column">
                <input class="input is-medium" type="text" placeholder="CPF do cliente">
                <button class="button is-primary mt-5">Buscar cliente</button>
            </div>
            <div class="column">
                 <input class="input is-medium" type="text" placeholder="Nome do cliente pesquisado" disabled>
            </div>
        </div>

        <hr>
        <h3>Dados do produto</h3>
        <hr>
        <div class="columns">
            <div class="column">
                <p>Código</p>
                <input type="number" class="input" placeholder="Código">
                <button class="button is-primary mt-5">Pesquisar</button>
            </div>
            <div class="column">
                <p>Produto</p>
                <input type="text" class="input" placeholder="Produto">
            </div>
              <div class="column">
                <p>Preço</p>
                <input type="text" class="input" placeholder="Preço">
            </div>
             <div class="column">
                <p>Qtd</p>
                <input type="text" class="input" placeholder="Quantidade">
                <button class="button is-info mt-5">Adicionar item</button>
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
                    <tr>
                        <td>1</td>
                        <td>Leite 1L piracanjuba</td>
                        <td>10:</td>
                        <td>5,50</td>
                        <td>89,80</td>
                    </tr>
                </tbody>
            </table>
            </div>
            <div class="column">
            <p>Total da venda:</p>
               <input class="input is-medium" type="text" placeholder="Total da venda" disabled>
               <button class="button is-primary mt-3" @click="showModal()">Pagar</button>
            </div>
        </div>
    <div :class="{modal: true, 'is-active': showModalPayment}">
        <div class="modal-background"></div>
        <div class="modal-content" style="overflow: hidden;">
            <div class="columns">
                <div class="column p-5">
                    <p>Dinheiro</p>
                    <input type="number" class="input" placeholder="Dinheiro">
                    <p>Cartão</p>
                    <input type="number" class="input" placeholder="Cartão">
                    <p>Cheque</p>
                    <input type="number" class="input" placeholder="Cheque">
                     <p>Troco</p>
                    <input type="text" class="input" placeholder="Troco" disabled>
                     <p>Total da venda</p>
                    <input type="text" class="input" placeholder="Total da Venda" disabled>
                    <button class="button is-primary mt-3">Finalizar Compra</button>
                </div>>
            </div>
        </div>
        <button class="modal-close is-large" @click="hideModal()"></button>
    </div>
    </div><!--Div pai-->
</template>

<script>
export default {
    data(){
        return {
            showModalPayment: false
        }
    },
    methods:{
        hideModal(){
            this.showModalPayment = false
        },
        showModal(){
            this.showModalPayment = true
        }
    }
}
</script>

<style scoped>
p, h1{
    color: white;
}
</style>