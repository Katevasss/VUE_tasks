<script setup>
import VolunteerTable from './components/VolunteerTable.vue'
</script>

<template>
  <div class="pageLayout">
    <header>Таблица учета волонтеров</header>

    <main>
      <VolunteerTable />
    </main>
  </div>
</template>

<style scoped>
.pageLayout {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}
header {
  font-weight: 600;
  text-transform: uppercase;
  padding: 2rem;
  border-radius: 5px;
  background-color: #d3e4f6;
}

@media (min-width: 1024px) {
  .pageLayout {
    display: flex;
    flex-wrap: wrap;
  }
}
</style>
