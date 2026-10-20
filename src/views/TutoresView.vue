<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';

const API_URL = 'http://localhost:3000';
const pets = ref([]);
const tutores = ref([]);
const loading = ref(true);

async function carregarDados() {
  loading.value = true;

  try {
    const [respostaPets, respostaTutores] = await Promise.all([
      fetch(`${API_URL}/pets`),
      fetch(`${API_URL}/tutores`),
    ]);

    if (!respostaPets.ok || !respostaTutores.ok) {
      throw new Error('Não foi possível carregar os dados.');
    }

    pets.value = await respostaPets.json();
    tutores.value = await respostaTutores.json();
  } catch (error) {
    console.error('Erro ao carregar dados:', error);
  } finally {
    loading.value = false;
  }
}

function quantidadePets(tutorId) {
  return pets.value.filter((pet) => pet.tutorId === tutorId).length;
}

function nomesPets(tutorId) {
  return pets.value
    .filter((pet) => pet.tutorId === tutorId)
    .map((pet) => pet.nome);
}

async function excluirTutor(id) {
  const confirmarExclusao = window.confirm('Deseja realmente excluir o tutor?');

  if (confirmarExclusao) {
    await fetch(`${API_URL}/tutores/${id}`, {
      method: 'DELETE',
    });

    await carregarDados();
  }
}

onMounted(carregarDados);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Tutores</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Tutores cadastrados no sistema.
      </p>
    </header>

    <RouterLink class="btn btn-primary mb-3" :to="{ name: 'addTutor' }">
      Adicionar Tutor
    </RouterLink>

    <p v-if="loading">Carregando tutores...</p>

    <table v-else class="table table-striped table-hover">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome Completo</th>
          <th>CPF / CNPJ</th>
          <th>Telefone / Celular / WhatsApp</th>
          <th>E-mail</th>
          <th>Pets (Quantidade)</th>
          <th>Nome dos Pets</th>
          <th>Ações</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="tutor in tutores" :key="tutor.id">
          <td>{{ tutor.id }}</td>
          <td>{{ tutor.nome }}</td>
          <td>{{ tutor.cpf }}</td>
          <td>{{ tutor.telefone }}</td>
          <td>{{ tutor.email }}</td>
          <td>{{ quantidadePets(tutor.id) }}</td>
          <td>{{ nomesPets(tutor.id).join(', ') || 'Nenhum pet cadastrado' }}</td>
          <td>
            <button class="btn btn-danger" @click="excluirTutor(tutor.id)">
              Excluir
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <p v-if="!loading" class="text-body-secondary mb-0">
      Exibindo {{ tutores.length }} tutores
    </p>
  </div>
</template>
