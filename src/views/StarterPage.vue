<script setup lang="ts">
import { ref, onMounted, toRefs, watchEffect } from 'vue'
import { useDessertStore } from '@/stores/starter'

// Acessa a store
const dessertStore = useDessertStore();
const { serverItems, totalItems, loading, loadItems } = toRefs(dessertStore);

// Referências para valores que são específicos da view
const itemsPerPage = ref(5)
const headers = ref([
  { title: 'Dessert (100g serving)', align: 'start', sortable: false, key: 'name' },
  { title: 'Calories', key: 'calories', align: 'end' },
  { title: 'Fat (g)', key: 'fat', align: 'end' },
  { title: 'Carbs (g)', key: 'carbs', align: 'end' },
  { title: 'Protein (g)', key: 'protein', align: 'end' },
  { title: 'Iron (%)', key: 'iron', align: 'end' },
])
const search = ref('')

watchEffect(() => {
  console.log("serverItems has changed:", serverItems.value);
});

// Carrega os dados na montagem do componente
onMounted(() => {
  dessertStore.loadItems({ page: 1, itemsPerPage: itemsPerPage.value, sortBy: [] })
})

</script>

<template>
  <v-data-table-server
    v-model:items-per-page="itemsPerPage"
    :headers="headers"
    :items="serverItems"
    :items-length="totalItems"
    :loading="loading"
    :search="search"
    item-value="name"
    @update:options="loadItems"
  ></v-data-table-server>
</template>
