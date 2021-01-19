<template>
<div>
        <h1 class="my-4">Cadastro de clientes</h1>
    <div class="container is-max-desktop">
        <div v-if="error != undefined">
            <div class="notification is-danger">{{error}}</div>
        </div>
        <div class="success">{{success}}</div>
        <div class="columns">
            <div class="column">
                <p>Nome:</p>
                <input class="input mb-5" type="text" placeholder="Nome" v-model="name">
                <p>CPF:</p>
                <input class="input" type="text" placeholder="CPF" v-model="cpf">
            </div>
            <div class="column">
                <p>E-mail:</p>
                <input class="input mb-5" type="text" placeholder="E-mail" v-model="email">
                <p>Telefone:</p>
                <input class="input" type="text" placeholder="Telefone" v-model="telephone">
            </div>
            <div class="column">
                <p>RG:</p>
                <input class="input mb-5" type="text" placeholder="RG" v-model="rg">
                <p>Celular:</p>
                <input class="input" type="text" placeholder="Celular" v-model="celphone">
            </div>
        </div>
        <div class="columns">
             <div class="column">
                 <p>Cidade:</p>
                <input class="input mb-5" type="text" placeholder="Cidade" v-model="city">
                <p>Número</p>
                <input class="input" type="text" placeholder="Número" v-model="number">
            </div>
            <div class="column">
                <p>Bairro</p>
                <input class="input mb-5" type="text" placeholder="Bairro" v-model="neighborhood">
                <p>Complemento:</p>
                <input class="input" type="text" placeholder="Complemento" v-model="complement">
            </div>
            <div class="column">
                <p>Endereço</p>
                <input class="input mb-5" type="text" placeholder="Endereço" v-model="address">
                <p>CEP:</p>
                <input class="input" type="text" placeholder="CEP" v-model="cep">
            </div>
            <div class="column">
                <div class="mb-6">
                    <p>UF:</p>
                <div class="select">
                    <select v-model="state">
                        <option>BA</option>
                        <option>SP</option>
                    </select>
                </div>
                </div>
                <div>
                 <button class="button is-primary" @click="register">Cadastrar Cliente</button>
                </div>

            </div>
        </div>
        <hr>
        <h2>Lista de Clientes</h2>
            <table class="table">
                <thead>
                    <tr>
                        <th>Código:</th>
                        <th>Nome:</th>
                        <th>Email:</th>
                        <th>Endereço:</th>
                        <th>Cidade</th>
                        <th>UF:</th>
                        <th>Cidade</th>
                        <th>CPF:</th>
                        <th>Ações</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="client in clients" :key="client.id">
                        <td>{{client.id}}</td>
                        <td>{{client.name}}</td>
                        <td>{{client.email}}:</td>
                        <td>{{client.email}}</td>
                        <td>{{client.city}}</td>
                        <td>{{client.state}}</td>
                        <td>{{client.city}}</td>
                        <td>{{client.cpf}}</td>
                        <td>
                            <button class="button is-primary" @click="register">Editar</button>
                            <button class="button is-danger" @click="register">Deletar</button>
                        </td>
                    </tr>
                </tbody>
            </table>
    </div><!--Div pai de todos-->
</div>
</template>

<script>
import  axios from 'axios';

    export default {
        created(){
            axios.get("http://localhost:5000/client").then(res=>{
                
                this.clients = res.data.result;
                console.log(this.clients);
            }).catch(err=>{
                console.log(err);
            })
        },
        data(){
            return{
                name: '',
                rg: '',
                cpf: '',
                email: '',
                telephone: '',
                celphone: '',
                cep: '',
                address: '',
                number: '',
                complement: '',
                neighborhood: '',
                city: '',
                state: '',
                error: undefined,
                success: '',
                clients: []

            }
        },
        methods: {
            register(){
                axios.post("http://localhost:5000/client", {
                    name: this.name,
                    rg: this.rg,
                    cpf: this.cpf,
                    email: this.email,
                    telephone: this.telephone,
                    celphone: this.celphone,
                    cep: this.cep,
                    address: this.address,
                    number: this.number,
                    complement: this.complement,
                    neighborhood: this.neighborhood,
                    city: this.city,
                    state: this.state
                }).then(res=>{
                    console.log(res);
                    this.success = 'Usuario cadastrado com sucesso';
                }).catch(err=>{
                    var msgError = err.response.data
                    this.error = msgError
                })
                //console.log(this.name);
            }
        }
    }

</script>

<style scoped>

.success{
    color: green;
}

</style>