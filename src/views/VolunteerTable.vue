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
    <CustomTable :columns="columns" :filteredVolunteers="filteredVolunteers">
      <template #name="{ volunteer }">
        {{ volunteer.first_name }} {{ volunteer.last_name }}
      </template>
      <template #medicalContraindications="{ volunteer }">
        {{ volunteer.medicalContraindications.join(', ') }}
      </template>
      <template #allows="{ volunteer }">
        <div v-if="volunteer.age >= 18">Не требуется</div>
        <div v-else>
          <CustomButton v-if="!isConfirmed(volunteer)" @click="showModal(volunteer)"
            >Подтвердить</CustomButton
          >
          <div v-else>Получено</div>
        </div>
      </template>
    </CustomTable>

    <div v-if="modalVisible" class="modal" @click.self="closeModal">
      <div class="modalContent">
        <button class="closeButton" @click="closeModal">&times;</button>
        <p>Разрешение от родителей/опекунов на волонтерство получено</p>
        <div class="buttonsLayout">
          <CustomButton :onClick="confirmPermission">Подтвердить</CustomButton>
          <CustomButton :onClick="closeModal">Отмена</CustomButton>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import CustomTable from '../components/CustomTable.vue'
import CustomButton from '../components/CustomButton.vue'

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
const modalVisible = ref(false)
const currentVolunteer = ref(null)
const columns = [
  {
    label: 'ФИО',
    key: 'name'
  },
  {
    label: 'Возраст',
    key: 'age'
  },
  {
    label: 'Направление',
    key: 'directions'
  },
  {
    label: 'Руководитель',
    key: 'leader'
  },
  {
    label: 'Мед. противопоказания',
    key: 'medicalContraindications'
  },
  {
    label: 'Разрешения',
    key: 'allows'
  }
]
const closeModal = () => (modalVisible.value = false)

const isConfirmed = (volunteer) => {
  return volunteer.allows === 'Получено'
}
const showModal = (volunteer) => {
  currentVolunteer.value = volunteer
  modalVisible.value = true
}
const confirmPermission = () => {
  currentVolunteer.value.allows = 'Получено'
  closeModal()
}

const filteredVolunteers = computed(() => {
  return volunteers.value.filter((volunteer) => {
    const fullName = volunteer.first_name.toLowerCase() + volunteer.last_name.toLowerCase()
    const matchesName = fullName.includes(searchQuery.value.toLowerCase())
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

.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modalContent {
  background: white;
  padding: 1.7rem;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
.closeButton {
  position: absolute;
  top: 0.6rem;
  right: 0.6rem;
  border-radius: 50%;
  background-color: #d3e4f6;
  border: none;
  font-size: 1rem;
  cursor: pointer;
}
.buttonsLayout {
  display: flex;
  justify-content: space-between;
}
</style>
