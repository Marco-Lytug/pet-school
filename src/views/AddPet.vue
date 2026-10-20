<script setup>
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';

const novoPet = ref({
  nome: '',
  especie: '',
  tutorId: '',
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
const tutores = ref([]);

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

async function salvarPet() {
  try {
    const resposta = await fetch(`${API_URL}/pets`, {
      method: 'POST',
      headers: {
        'Content-type': 'application/json',
      },
      body: JSON.stringify(novoPet.value),
    });

    if (!resposta.ok) {
      throw new Error('Não foi possível cadastrar o pet.');
    }

    router.push('/pets');
  } catch (error) {
    console.error('Erro ao salvar pet:', error);
    erro.value = 'Não foi possível cadastrar o pet.';
  }
}

onMounted(carregarTutores);
</script>

<template>
 <div>
 <header class="mb-4">
 <h1 class="text-2xl font-bold">Listagem de Pets</h1>
 <p class="text-body-secondary mb-0">Cadastro de Pets no sistema.</p>
 </header>

 <p v-if="carregandoTutores">
   Carregando Tutores...
 </p>
 <div v-else>
   <p v-if="erro" class="alert alert-danger" role="alert">
     {{ erro }}
   </p>
 </div>

 <form type="submit" @submit.prevent="salvarPet">

 <div class="col-md-3"> <!-- coluna e tamanho da coluna -->
 <label for="nome" class="from-label">
 Nome do Pet
 </label>

 <input type="text"
 id="nome"
 v-model="novoPet.nome"
 class="form-control"
 required>
 </div>

 <div class="col-md-3">
 <label for="nome" class="from-label">
 Espécie
 </label>

 <select id="especie" v-model="novoPet.especie" class="form-select" required>
 <option value="" disabled>
 Selecione a espécie
 </option>

 <option value="Cachorro" >
 Cachorro
 </option>

 <option value="Gato" >
 Gato
 </option>
 </select>

 </div>

 <div class="col-md-3">
 <label for="nome" class="from-label">
 Tutor
 </label>

 <select id="tutor" v-model="novoPet.tutorId" class="form-select" required>
 <option value="" disabled>
 Selecione um tutor
 </option>

 <option v-for="tutor in tutores" :key="tutor.id" :value="tutor.id">
 {{ tutor.nome }}
 </option>
 </select>

 </div>



 <button type="submit" class="btn btn-primary">Adicionar pet</button>

 </form>

 </div>
</template>

