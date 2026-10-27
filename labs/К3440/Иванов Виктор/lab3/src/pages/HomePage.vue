<script setup>
import { ref, computed, onMounted } from 'vue';
import RestaurantCard from '../components/RestaurantCard.vue';
import SearchFilters from '../components/SearchFilters.vue';
import { useApi } from '../composables/useApi';
const { api, message } = useApi();
const restaurants = ref([]);
const error = ref('');
const loading = ref(true);
const filters = ref({ query: '', cuisine: '', district: '', price: 0 });
const found = computed(() => restaurants.value.filter(r => {
  const f = filters.value;
  const term = f.query.trim().toLocaleLowerCase('ru');
  return (!term || `${r.name} ${r.description}`.toLocaleLowerCase('ru').includes(term))
    && (!f.cuisine || r.cuisine === f.cuisine)
    && (!f.district || r.district === f.district)
    && (!f.price || r.price <= f.price);
}));
onMounted(async () => { try { restaurants.value = (await api.get('/restaurants')).data; } catch (e) { error.value = message(e); } finally { loading.value = false; } });
</script>
<template>
  <section class="hero mb-5"><p class="eyebrow">Ваш вечер начинается здесь</p><h1>Хорошие встречи случаются за столом</h1><p class="lead">Откройте новое любимое место, выберите удобное время и забронируйте столик за пару минут.</p><a class="btn btn-primary btn-lg" href="#search">Искать рестораны</a></section>
  <section id="search" aria-labelledby="search-title">
    <div class="d-flex flex-wrap justify-content-between align-items-end gap-2 mb-3"><div><p class="eyebrow mb-1">Подборка для вас</p><h2 id="search-title" class="section-title mb-0">Найдите свой ресторан</h2></div><p class="mb-0" aria-live="polite">Найдено ресторанов: {{ found.length }}</p></div>
    <SearchFilters v-model="filters" />
    <p v-if="loading" role="status">Загрузка ресторанов…</p>
    <p v-else-if="error" class="alert alert-danger" role="alert">Не удалось загрузить рестораны: {{ error }}. Запустите API-сервер.</p>
    <p v-else-if="!found.length" class="empty">Ничего не найдено. Попробуйте изменить фильтры.</p>
    <div v-else class="row g-4"><div v-for="restaurant in found" :key="restaurant.id" class="col-md-6 col-xl-4"><RestaurantCard :restaurant="restaurant" /></div></div>
  </section>
</template>
