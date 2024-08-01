<template>
  <table>
    <thead>
      <tr>
        <th v-for="column in props.columns" :key="column.key">{{ column.label }}</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="volunteer in props.filteredVolunteers"
        :key="volunteer.last_name + volunteer.first_name"
      >
        <td v-for="column in props.columns" :key="column.key">
          <slot :name="column.key" :volunteer="volunteer">
            {{ volunteer[column.key] }}
          </slot>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  columns: {
    type: Array
  },
  filteredVolunteers: {
    type: Array
  }
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
