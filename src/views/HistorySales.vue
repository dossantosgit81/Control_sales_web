<template>
 <div>
     <h1 class="notification is-info">Histórico de vendas</h1>
     <div class="container is-flex">
         <div class="form-warp">
            <div class="control">
                <p>Data inicial:</p>
                <input type="text" class="input">
            </div>
            <div class="control pt-5">
                <p>Data final</p>
                <input type="text" class="input">
            </div>
            <div class="control pt-5">
                <button class="button is-primary">Pesquisar vendas por periodo</button>
            </div>
         </div>
         <table class="table ml-6">
             <thead>
                 <tr>
                    <th>Código</th>
                    <th>Data da venda</th>
                    <th>Cliente</th>
                    <th>Total da venda</th>
                    <th>Observações</th>
                 </tr>
             </thead>
             <tbody>
                 <tr>
                     <td>1</td>
                     <td>2018-01-01 02:01:01</td>
                     <td>Rafael Mendes</td>
                     <td>25.693</td>
                     <td>O cliente é do bem</td>
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
export default {
    
}
</script>

<style scoped>

</style>