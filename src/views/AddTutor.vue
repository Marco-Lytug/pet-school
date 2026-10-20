<script setup>
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';

const novoTutor = ref({
  nome: '',
  telefone: '',
  id: '',
  cpf: '',
  data: '',
  email: '',
  rua: '',
  bairro: '',
  estado: '',
  numero: '',
  cidade: '',
  cep: '',
});

const router = useRouter();
const carregandoTutores = ref(false);
const erro = ref('');

// aqui estou declarando a api que vai retornar a listagem do todos os pets
// sera responsavel por cadastrar novos pets
// POST para cadastrar novos pets salvar qualquer coisa
//GET (chamar todos)

/* MÉTODOS HTTP

 GET: recuperar id
 POST: salvar
 PH/PATCH : atualizar id
 DELETE: remover id
*/

const API_URL = "http://localhost:3000";

//saber todos os tutores que existem
const tutores = ref({});

async function carregarTutores() {
  carregandoTutores.value = true;
  erro.value = '';

  try {
    const resposta = await fetch(`${API_URL}/tutores`);
    if (!resposta.ok) {
      throw new Error('Não foi possível carregar os tutores.');
    }

    tutores.value = await resposta.json();
  } catch (error) {
    console.error('Erro ao carregar tutores:', error);
    erro.value = 'Não foi possível carregar os tutores.';
  } finally {
    carregandoTutores.value = false;
  }
}

async function salvarTutor() {
  try {
    const resposta = await fetch(`${API_URL}/tutores`, {
      method: 'POST',
      headers: {
        'Content-type': 'application/json',
      },
      body: JSON.stringify(novoTutor.value),
    });

    if (!resposta.ok) {
      throw new Error('Não foi possível cadastrar o tutor.');
    }

    router.push('/tutores');
  } catch (error) {
    console.error('Erro ao salvar tutor:', error);
    erro.value = 'Não foi possível cadastrar o tutor.';
  }
}

async function buscarCEP() {
 const cep = novoTutor.value.cep.replace(/\D/g, '');
 if (cep.length !== 8) {
 return;
 }
 try {
 const resposta = await fetch(
 `https://viacep.com.br/ws/${cep}/json/`
 );
 if (!resposta.ok) {
 throw new Error('Erro ao consultar o CEP');
 }

 const dados = await resposta.json();

 if (dados.erro) {
 alert('CEP não encontrado!');
 return;
 }
 novoTutor.value.rua = dados.logradouro;
 novoTutor.value.bairro = dados.bairro;
 novoTutor.value.cidade = dados.localidade;
 novoTutor.value.estado = dados.uf;

 } catch (error) {
 console.error('Erro ao buscar CEP:', error);
 alert('Não foi possível consultar o CEP.');
 }
}




onMounted(carregarTutores);
</script>

<template>
 <div>
 <header class="mb-4">
 <h1 class="text-2xl font-bold">Cadastar Tutores</h1>
 <p class="text-body-secondary mb-0">Preencha os dados para cadastrar um novo tutor</p>
 </header>


 <p v-if="carregandoTutores">
 Carregando Tutores...
 </p>
 <div v-else>
 <p v-if="erro" class="alert alert-danger" role="alert">
 {{ erro }}
 </p>
 </div>

 <form type="submit" @submit.prevent="salvarTutor">

 <div>
 <th>
 Dados do Tutor
 </th>
 <div class="col-md-3">
 <th>
 <label for="id" class="from-label" >
 ID
 </label>
 </th>


 <input type="tel"
 id="id"
 v-model="novoTutor.id"
 class="form-control"
 placeholder="Gerado automaticamente"
 disabled>
 </div>


 <div class="col-md-3">
 <th>
 <label for="nome" class="from-label" >
 Nome Completo
 </label>
 </th> <!-- coluna e tamanho da coluna -->


 <input type="text"
 id="nome"
 v-model="novoTutor.nome"
 class="form-control"
 placeholder="Digite o nome completo"
 required>
 </div>

 <div class="col-md-3">
 <th>
 <label for="telefone" class="from-label" >
 Telefone / Celular/ WhatsApp
 </label>
 </th>


 <input type="tel"
 id="telefone"
 v-model="novoTutor.telefone"
 class="form-control"
 placeholder="(00) 00000-0000"
 required>
 </div>

 </div>

 <div>
 <div class="col-md-3">
 <th>
 <label for="cpf" class="from-label" >
 CPF / CNPJ
 </label>
 </th>


 <input type=""
 id="cpf"
 v-model="novoTutor.cpf"
 class="form-control"
 placeholder="Digite o CPF ou CNPJ"
 required>
 </div>

 <div class="col-md-3">
 <th>
 <label for="data" class="from-label" >
 Data de Nascimento
 </label>
 </th>


 <input type="month"
 id="data"
 v-model="novoTutor.data"
 class="form-control"
 placeholder="dd/mm/aa"
 required>
 </div>

 <div class="col-md-3">
 <th>
 <label for="data" class="from-label" >
 Email
 </label>
 </th>


 <input type="email"
 id="email"
 v-model="novoTutor.email"
 class="form-control"
 placeholder="nome@example.com"
 required>
 </div>



 </div>

 <th>
 Endereço
 </th>
 <div class="col-md-3">
 <th>
 <label for="rua" class="from-label" >
 Rua
 </label>
 </th>


 <input type="text"
 id="rua"
 v-model="novoTutor.rua"
 class="form-control"
 placeholder="Digite o nome da rua"
 required>
 </div>

 <div class="col-md-3">
 <th>
 <label for="bairro" class="from-label" >
 Bairro
 </label>
 </th>


 <input type="text"
 id="bairro"
 v-model="novoTutor.bairro"
 class="form-control"
 placeholder="Digite o bairro"
 required>
 </div>

 <div class="col-md-3">
 <th>
 <label for="numero" class="from-label" >
 Número
 </label>
 </th>


 <input type="number"
 id="numero"
 v-model="novoTutor.numero"
 class="form-control"
 placeholder="Número"
 required>
 </div>

 <div class="col-md-3">
 <th>
 <label for="cidade" class="from-label" >
 Cidade
 </label>
 </th>


 <input type="text"
 id="cidade"
 v-model="novoTutor.cidade"
 class="form-control"
 placeholder="Digite a cidade"
 required>
 </div>

 <div class="col-md-3">
 <th>
 <label for="cep" class="from-label" >
 CEP
 </label>
 </th>


 <input type="text"
 id="cep"
 v-model="novoTutor.cep"
 @blur="buscarCEP"
 class="form-control"
 placeholder="00000-000"
 maxlength="9"
 required>
 </div>

<button type="submit" class="btn btn-primary">Cadastrar Tutor</button>

 </form>

 </div>





 </template>
