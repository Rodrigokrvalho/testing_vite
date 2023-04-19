<template>
  <h3>Pessoa</h3>
  <section>
    <h4>Salvar</h4>
    <label>
      Nome
      <input
        type="text"
        v-model="name"
      >
    </label>

    <label>
      Idade
      <input
        type="number"
        v-model="age"
      >
    </label>
    <br>
    <button @click="sendPerson">Enviar</button>
  </section>

  <section>
    <h4>Listar</h4>

    <p>{{ persons }}</p>

    <button @click="getPersons">Listar</button>
  </section>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

interface Person {
  name: string;
  age: number;
}

const routes = {
  person: 'http://localhost:3000/person'
};

const name = ref('');
const age = ref('');
const persons = ref<Person[]>([]);

function sendPerson(): void {
  if (!name || !age) {
    alert("campos nao preenchidos na pessoa");
  }

  const payload = {
    name,
    age
  };

  const response = axios.post(routes.person, payload).then(response => response.data);

  alert('RESPOSTA: ' + response);
}

function getPersons() {
  persons.value = [];

  axios.get(routes.person).then(response => {
    if (!response.data.length) {
      alert('sem usuarios');
      return;
    }

    persons.value = response.data;
  });
}
</script>


<style scoped>
input {
  margin: 1rem;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
