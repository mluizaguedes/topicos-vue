<template>
  <div>
    {{ nome }} - {{ senha }}
    <p>
      <label for="nome">Nome: </label>
      <input id="nome" type="text" v-model="nome" />
      <input id="nome2" type="text" :value="nome" />
    </p>
    <p>
      <label for="senha">Senha: </label>
      <input id="senha" type="text" v-model="senha" />
    </p>

    <button @click="inserirUsuario">Inserir</button>

    <table>
      <thead>
        <tr>
          <th>Id</th>
          <th>Nome</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="usuario in usuarios" :key="usuario.id">
          <th>{{ usuario.id }}</th>
          <th>{{ usuario.nome }}</th>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface usuario {
  id?: number
  nome: string
  senha: string
}

const nome = ref<string>('')
const senha = ref<string>('')
const usuarios = ref<usuario[]>([
  { id: 1, nome: 'João', senha: '123' },
  { id: 2, nome: 'Maria', senha: '1234' },
])

function inserirUsuario() {
  const id = usuarios.value.length + 1
  usuarios.value.push({ id, nome: nome.value, senha: senha.value })
}
</script>
