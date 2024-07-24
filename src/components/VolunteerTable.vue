<template>
  <div class="tableLayout">
    <div class="inputContainer">
      <input
        class="searchInput"
        type="text"
        v-model="searchQuery"
        placeholder="Поиск по имени волонтера"
      />
      <label class="labelContainer">
        <input class="checkbox" type="checkbox" v-model="filterMinors" />
        Показать только несовершеннолетних
      </label>
    </div>

    <table>
      <thead>
        <tr>
          <th>ФИО</th>
          <th>Возраст</th>
          <th>Направление</th>
          <th>Руководитель</th>
          <th>Медицинские противопоказания</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="volunteer in filteredVolunteers"
          :key="volunteer.last_name + volunteer.first_name"
        >
          <td>{{ volunteer.first_name }} {{ volunteer.last_name }}</td>
          <td>{{ volunteer.age }}</td>
          <td>{{ volunteer.directions }}</td>
          <td>{{ volunteer.leader }}</td>
          <td>{{ volunteer.medicalContraindications.join(', ') }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const volunteers = ref([
  {
    last_name: 'Иванов',
    first_name: 'Иван',
    age: 22,
    directions: 'Кухня',
    leader: 'Ольга Кузьмина',
    medicalContraindications: []
  },
  {
    last_name: 'Вересова',
    first_name: 'Елена',
    age: 16,
    directions: 'Кухня',
    leader: 'Ольга Кузьмина',
    medicalContraindications: ['поллиноз', 'астма']
  },
  {
    last_name: 'Рубцов',
    first_name: 'Иван',
    age: 32,
    directions: 'КПП',
    leader: 'Андрей Андреев',
    medicalContraindications: ['гастрит']
  },
  {
    last_name: 'Петров',
    first_name: 'Петр',
    age: 29,
    directions: 'Освещение',
    leader: 'Андрей Андреев',
    medicalContraindications: ['']
  },
  {
    last_name: 'Федоровна',
    first_name: 'Мария',
    age: 17,
    directions: 'Декорации',
    leader: 'Ирина Аллегрова',
    medicalContraindications: ['циклотомия']
  }
])

const searchQuery = ref('')
const filterMinors = ref(false)

const filteredVolunteers = computed(() => {
  return volunteers.value.filter((volunteer) => {
    const matchesName = volunteer.first_name.toLowerCase().includes(searchQuery.value.toLowerCase())
    const isMinor = !filterMinors.value || volunteer.age < 18
    return matchesName && isMinor
  })
})
</script>

<style>
.tableLayout {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}
.inputContainer {
  display: flex;
  justify-content: space-between;
  gap: 2rem;
}
.searchInput {
  flex: 1;
}
.labelContainer {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
.checkbox {
  cursor: pointer;
  padding: 0.7rem;
}
input {
  padding: 1rem;
  border: none;
  box-shadow: 1px 1px 5px 1px gray;
  border-radius: 5px;
}
input[type='text']:focus {
  border: 3px solid #d3e4f6;
  outline: 0;
}
input[type='checkbox'] {
  border-radius: 5px;
  appearance: none;
}

input[type='checkbox']:checked::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
}
input[type='checkbox']:checked::after {
  content: '✓';
  position: absolute;
  left: 6px;
  top: 50%;
  transform: translateY(-50%);
}

input[type='checkbox']:checked {
  position: relative;
  background-color: #d3e4f6;
}
table {
  width: 100%;
  border-collapse: collapse;
  box-shadow: 3px 3px 10px 3px gray;
}
th,
td {
  padding: 1rem;
  text-align: left;
}

th {
  font-weight: 600;
  border-bottom: 1px solid gray;
  background-color: #d3e4f6;
}
</style>
