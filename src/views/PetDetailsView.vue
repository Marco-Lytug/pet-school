<script setup>
import { onMounted, ref } from 'vue';
import { useRoute, RouterLink } from 'vue-router';

const route = useRoute();
const API_URL = 'http://localhost:3000';

const pet = ref({});
const tutor = ref({});

async function carregarPets() {
  const idPet = route.params.id;

  const respostaPet = await fetch(`${API_URL}/pets/${idPet}`);
  pet.value = await respostaPet.json();

  if (pet.value?.tutorId) {
    const respostaTutor = await fetch(`${API_URL}/tutores/${pet.value.tutorId}`);
    tutor.value = await respostaTutor.json();
  }
}

onMounted(carregarPets);
</script>

<template>
  <div>
    <h1>Nome do Pet: {{ pet.nome }}</h1>
    <p>Espécie: {{ pet.especie }}</p>
    <p>Nome do Tutor: {{ tutor.nome }}</p>

    <RouterLink class="btn btn-primary" :to="{ name: 'pets' }">
      Atualizar

    </RouterLink>

    <RouterLink class="btn btn-primary" :to="{ name: 'pets' }">
      Voltar
    </RouterLink>

  </div>
</template>

<style>

</style>
