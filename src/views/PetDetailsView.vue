<script setup>
import { onMounted,ref } from 'vue';
import { useRoute } from 'vue-router';
const pet = ref({});

const route = useRoute();
const API_URL = 'http://localhost:3000';
//exibir informaçãoes do pet e do tutor

const tutor = ref({});

async function carregarPet() {
 //exibindo dados do id do pet
 const idPet = route.params.id;
 console.log('ID do pet:', idPet);

 const respostaPet = await fetch (`${API_URL}/pets/${idPet}`);
 pet.value = await respostaPet.json();

 const respostaTutor = await fetch (
 `${API_URL}/tutores/${pet.value.tutorId}`,
 );
 tutor.value = await respostaTutor.json();
}

onMounted (carregarPet);
</script>

<template>
 <h1>Nome do Pet: {{ pet.nome }}</h1>
 <p>
 Espécie: {{ pet.especie }}
 </p>
 <p>
 Nome do tutor: {{ tutor?.nome }}
 </p>

 <button class="btn btn">
 <RouterLink :to="{name: 'pets'}" >
 Voltar
 </RouterLink>

 </button>
</template>
