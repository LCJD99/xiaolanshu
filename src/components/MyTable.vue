<script setup>
import axios from 'axios'
import { ref, onBeforeMount } from 'vue'

const url = 'http://localhost:3001/persons'

// 1. fetch data
const persons = ref([])
const aPerson = ref({
  "id": 1,
  "name": null,
  "age": null,
  "gender": null,
  "city": null,
  "occupytion": null,
})
const fetchPersons = async () => {
  try {
    const response = await axios.get(url);
    persons.value = response.data;
    aPerson.value.id = persons.value.length + 1
    console.log(response.data);
  } catch (error) {
    console.error("Failed to fetch student data:", error);
  }
}

const addPerson = async () => {
  try {
    aPerson.value.id = persons.value.length + 1
    await axios.post(url, aPerson.value)
    fetchPersons()
  } catch (error) {
    console.error("Failed to fetch student data:", error);
  }
}

const delatePerson = async () => {
  try {
    await axios.delete(url + "/" + aPerson.value.id)
    fetchPersons()
  } catch (error) {
    console.error("Failed to fetch student data:", error);
  }
}

const modifyPerson = async () => {
  try {
    await axios.put(url + "/" + aPerson.value.id, aPerson.value)
    fetchPersons()
  } catch (error) {
    console.error("Failed to fetch student data:", error);
  }
}

onBeforeMount(fetchPersons)

// 2. pass data to table

</script>
<template>
  <div>
    <h2>Infomation</h2>
    <button @click=fetchPersons>
      refresh
    </button>
    <table border="1">
      <thead>
        <tr>
          <th>id</th>
          <th>Name</th>
          <th>age</th>
          <th>gender</th>
          <th>city</th>
          <th>occupation</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(person, index) in persons" :key="index">
          <td>{{ person.id }}</td>
          <td>{{ person.name }}</td>
          <td>{{ person.age }}</td>
          <td>{{ person.gender }}</td>
          <td>{{ person.city }}</td>
          <td>{{ person.occupation }}</td>
        </tr>
      </tbody>
    </table>
    <div>
      <p>id</p>
      <input v-model="aPerson.id">
    </div>
    <div>
      <p>name</p>
      <input v-model="aPerson.name">
    </div>
    <div>
      <p>age</p>
      <input v-model="aPerson.age">
    </div>
    <div>
      <p>gender</p>
      <input v-model="aPerson.gender">
    </div>
    <div>
      <p>city</p>
      <input v-model="aPerson.city">
    </div>
    <div>
      <p>occupation</p>
      <input v-model="aPerson.occupation">
    </div>
    <button @click=addPerson class="inline-btn">
      add
    </button>
    <button @click=delatePerson class="inline-btn">
      delete
    </button>
    <button @click=modifyPerson class="inline-btn">
      modify
    </button>
  </div>
</template>

<style scoped>
.inline-btn {
  display: inline-block;
  margin-right: 8px;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  padding: 8px;
  text-align: left;
  border: 1px solid #ddd;
}
</style>
