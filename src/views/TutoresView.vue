<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';
const API_URL = 'http://localhost:3000';
const tutores = ref([]);
const loading = ref(true);


async function carregarDados() {
  const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();

  // tutores.value = (await respostaTutores.json()).map((tutor) => ({
  //   ...tutor,
  //   id: Number(tutor.id),
  // }));

  console.table('tutores', tutores.value);


  loading.value = false;
}


async function excluirTutor(id) {
  const confirmarExclusao = window.confirm('Deseja realmente excluir o tutor?');

  if(confirmarExclusao){
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

    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addTutor' }"
    >
      Adicionar Tutores
    </RouterLink>

    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Número de Telefone</th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="tutor in tutores"
          :key="tutor.id"
        >
          <td>{{ tutor.id }}</td>
          <td>{{ tutor.nome }}</td>
          <td>{{ tutor.telefone }}</td>
          <td>
            <RouterLink :to="`/tutores/${tutor.id}`">Visualizar</RouterLink>
             <button class="btn btn-danger" @click="excluirTutor(tutor.id)">Excluir</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
