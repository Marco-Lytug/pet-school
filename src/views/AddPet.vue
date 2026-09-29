<script setup>
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';

const API_URL = 'http://localhost:3000';
const router = useRouter();

const novoPet = ref({
  nome: '',
  especie: '',
  tutorId: '',
});

const tutores = ref([]);

async function carregarTutores() {
  const resposta = await fetch(`${API_URL}/tutores`);
  tutores.value = await resposta.json();
}

async function salvarPet() {
  const resposta = await fetch(`${API_URL}/pets`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      ...novoPet.value,
      tutorId: Number(novoPet.value.tutorId),
    }),
  });

  if (!resposta.ok) {
    throw new Error('Não foi possível salvar o pet.');
  }

  router.push({ name: 'pets' });
}

onMounted(carregarTutores);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastro de Pets</h1>
      <p class="text-body-secondary mb-0">Cadastro de Pets no sistema.</p>
    </header>

    <form class="row g-3" @submit.prevent="salvarPet">
      <div class="col-md-6">
        <label for="nome" class="form-label">Nome do Pet</label>
        <input
          id="nome"
          v-model="novoPet.nome"
          type="text"
          class="form-control"
          placeholder="Digite o nome do pet"
          required
        />
      </div>

      <div class="col-md-6">
        <label for="especie" class="form-label">Espécie</label>
        <select id="especie" v-model="novoPet.especie" class="form-select" required>
          <option value="" disabled selected>Selecione a espécie</option>
          <option value="Cachorro">Cachorro</option>
          <option value="Gato">Gato</option>
          <option value="Pássaro">Pássaro</option>
          <option value="Outro">Outro</option>
        </select>
      </div>

      <div class="col-md-6">
        <label for="tutor" class="form-label">Tutor</label>
        <select id="tutor" v-model="novoPet.tutorId" class="form-select" required>
          <option value="" disabled selected>Selecione o tutor</option>
          <option v-for="tutor in tutores" :key="tutor.id" :value="tutor.id">
            {{ tutor.nome }}
          </option>
        </select>
      </div>

      <div class="col-12">
        <button type="submit" class="btn btn-primary">Salvar Pet</button>
        <button type="button" class="btn btn-secondary ms-2" @click="router.push({ name: 'pets' })">
          Cancelar
        </button>
      </div>
    </form>
  </div>
</template>
