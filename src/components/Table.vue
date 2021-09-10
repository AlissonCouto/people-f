<template>
    <table class="table table-striped">
                        <thead class="thead-dark">
                            <tr>
                                <th>Nome Completo</th>
                                <th>Email</th>
                                <th>Telefone</th>
                                <th>Pessoa Jurídica</th>
                                <th>CPF</th>
                                <th>CNPJ</th>
                                <th>Ação</th>
                            </tr>
                        </thead>

                        <tbody>
                            <tr v-for="(item, index) in this.people" :key="index">
                                <td>{{fullName(item)}}</td>
                                <td>{{item.email}}</td>
                                <td>{{item.phone}}</td>
                                <td><span class="tg-legal-person" :class="item.legalPerson ? 'bg-success' : 'bg-danger' ">{{item.legalPerson ? 'Sim' : 'Não'}}</span></td>
                                <td>{{item.cpf}}</td>
                                <td>{{item.cnpj}}</td>
                                <td>
                                    <button @click="edit(index)" class="btn btn-info">Editar</button>
                                    <button @click="remove(index)" class="btn btn-danger">Excluir</button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
</template>

<script>
export default {
    props: ['people'],
    computed: {
      fullName(){
          return (item) => item.firstName + " " + item.lastName;
      }
    },
    methods:{
        edit(index){
            this.$emit('edit', index);
        },
        remove(index){
            this.$emit('remove', index);
        }
    }
}
</script>

<style>
    .tg-legal-person{
        color: #fff;
        padding: 5px;
    }
</style>
