<script setup>
import { RouterLink } from 'vue-router';
defineProps({ restaurant: { type: Object, required: true } });
</script>
<template>
  <article class="restaurant-card">
    <div class="restaurant-art" :style="{ '--art': restaurant.color }" role="img" :aria-label="`Иллюстрация ресторана ${restaurant.name}`">{{ restaurant.emoji }}</div>
    <div class="restaurant-body">
      <div class="d-flex justify-content-between gap-2"><h3>{{ restaurant.name }}</h3><span :aria-label="`Рейтинг ${restaurant.rating} из 5`">★ {{ restaurant.rating }}</span></div>
      <p class="meta mb-2">{{ restaurant.cuisine }} · {{ restaurant.district }} · {{ '₽'.repeat(restaurant.price) }}</p>
      <p class="mb-3">{{ restaurant.description }}</p>
      <RouterLink class="btn btn-outline-primary" :to="`/restaurants/${restaurant.id}`">Подробнее о ресторане {{ restaurant.name }}</RouterLink>
    </div>
  </article>
</template>
