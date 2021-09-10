<template>
  <div class="container">
    <div class="row">
      <h1>Cadastro de pessoas</h1>
    </div>

    <div class="row">
      <div class="col-md-8 mx-md-auto">
        <form class="col-12">

          <div class="form-group">

            <label for="first-name">Nome</label>
            <input type="text" class="form-control" id="first-name" v-model="form.firstName" autofocus>

          </div> <!-- FIELD -->

          <div class="form-group">

            <label for="last-name">Sobrenome</label>
            <input type="text" class="form-control" id="last-name" v-model="form.lastName">

          </div> <!-- FIELD -->

          <div class="form-group">

            <label for="email">Email</label>
            <input type="email" class="form-control" id="email" v-model="form.email">

          </div> <!-- FIELD -->

          <div class="form-group">

            <label for="phone">Telefone</label>
            <input type="tel" class="form-control" id="phone" v-model="form.phone">

          </div> <!-- FIELD -->

          <div class="form-group form-check">

            <input type="checkbox" class="form-check-input" id="legal-person" v-model="form.legalPerson">
            <label class="form-check-label" for="legal-person">Pessoa Jurídica</label>

          </div> <!-- FIELD -->

          <div class="form-group" v-if="!form.legalPerson">

            <label for="cpf">CPF</label>
            <input type="text" class="form-control" id="cpf" v-model="form.cpf">
            <span v-if="error.cpf" class="font-weight-bold text-danger filter_validate">O CPF é obrigatório.</span>

          </div> <!-- FIELD -->

          <div class="form-group" v-if="form.legalPerson">

            <label for="cnpj">CNPJ</label>
            <input type="text" class="form-control" id="cnpj" v-model="form.cnpj">
            <span v-if="error.cnpj" class="font-weight-bold text-danger filter_validate">O CNPJ é obrigatório.</span>

          </div> <!-- FIELD -->

          <button @click="save($event)" class="btn btn-success">Salvar</button>
        </form>
      </div>
    </div> <!-- Form row -->

    <div class="row">
        <div class="col-12 mt-5 mx-md-auto">

            <Table v-on:edit="this.edit" v-on:remove="this.remove" :people="this.people"></Table>

        </div>
    </div> <!-- Table row -->

  </div>
</template>

<script>
import Table from './components/Table.vue'

export default {
  name: 'App',
  components: {
    Table
  },
  data() {
    return {
        form: {
          id: 0,
          firstName: '',
          lastName: '',
          email: '',
          phone: '',
          legalPerson: false,
          cpf: '',
          cnpj: '',
        },
        people: [],
        selectId: 0,
        error: {
          cpf: false,
          cnpj: false
        }
    }
  },
  methods: {
      save(e){

          e.preventDefault();

          // Validation of CPF and CNPJ fields

          if(this.form.legalPerson && this.form.cnpj.length < 1){
            this.error.cnpj = true;
            this.error.cpf = false;
          }else if((!this.form.legalPerson) && this.form.cpf.length < 1){
            this.error.cnpj = false;
            this.error.cpf = true;
          }else{

            var person = {
                firstName: this.form.firstName,
                lastName: this.form.lastName,
                email: this.form.email,
                phone: this.form.phone,
                legalPerson: this.form.legalPerson,
                cpf: this.form.cpf,
                cnpj: this.form.cnpj,
              };

              if(this.form.id === 0){
                  person.id = this.people.length + 1;
          
                  this.people.push(person);
              }else{
                  this.people[this.selectId] = person;
              }

              this.resetForm();

        }// Validation of CPF and CNPJ fields

      },
      remove(index){
          this.people.splice(index, 1);
      },
      edit(index){
          this.selectId = index;

          let item = this.people[index];
          this.form = Object.assign({}, item);
      },
      resetForm(){
          this.form = {
              id: 0,
              firstName: '',
              lastName: '',
              email: '',
              phone: '',
              legalPerson: '',
              cpf: '',
              cnpj: '',
          };
      },
      },
}
</script>
