<script setup>
import { RouterLink } from 'vue-router';
</script>


<template>
  <nav>
    <div class="flex-column nav=pills gap-1">
      <Router-Link to="/pets">
        <i class="bi bi-heart me-2">
        </i>
        Pets
      </Router-Link>
 <Router-Link to="/pets">
        <i class="bi bi-plus-circle me-2">
        </i>
        Novo Pet
      </Router-Link>
    </div>
  </nav>
</template>
