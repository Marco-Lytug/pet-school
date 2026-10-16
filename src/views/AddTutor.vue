<script setup>
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';

const API_URL = 'http://localhost:3000';
const router = useRouter();

const novoTutor = ref({
  nome: '',
  telefone: '',
});

const tutores = ref([]);

async function carregarTutores() {
  const resposta = await fetch(`${API_URL}/tutores`);
  tutores.value = await resposta.json();
}

async function salvarTutor() {
  const resposta = await fetch(`${API_URL}/tutores`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      ...novoTutor.value,
    }),
  });

  if (!resposta.ok) {
    throw new Error('Não foi possível salvar o tutor.');
  }

  router.push({ name: 'tutores' });
}

onMounted(carregarTutores);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastro de Tutores</h1>
      <p class="text-body-secondary mb-0">Cadastro de Tutores no sistema.</p>
    </header>

    <form class="row g-3" @submit.prevent="salvarTutor">
      <div class="col-md-6">
        <label for="nome" class="form-label">Nome do Tutor</label>
        <input
          id="nome"
          v-model="novoTutor.nome"
          type="text"
          class="form-control"
          placeholder="Digite o nome do tutor"
          required
        />
      </div>

      <div class="col-md-6">
        <label for="telefone" class="form-label">Número de Telefone</label>
        <input
          id="telefone"
          v-model="novoTutor.telefone"
          type="tel"
          class="form-control"
          placeholder="Digite o número de telefone do tutor"
          required
        />
      </div>


      <div class="col-12">
        <button type="submit" class="btn btn-primary">Salvar Tutores</button>
        <button type="button" class="btn btn-secondary ms-2" @click="router.push({ name: 'tutores' })">
          Cancelar
        </button>
      </div>
    </form>
  </div>
</template>
