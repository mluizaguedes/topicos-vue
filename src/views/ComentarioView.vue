<template>
  <div>
    <!-- cadastrar -->
    <form @submit.prevent="cadastrar">
      <p>
        <label id="conteudoLabel" for="conteudo">Conteúdo: </label>
        <input type="text" id="conteudo" v-model="novoComentario.conteudo" required />
      </p>
      <p>
        <label id="dataHoraLabel" for="dataHoraCriacao">Data/Hora: </label>
        <input
          type="datetime-local"
          id="dataHoraCriacao"
          v-model="novoComentario.dataHoraCriacao"
        />
      </p>
      <p>
        <label id="urgenciaLabel" for="urgencia">Urgência: </label>
        <input type="number" id="urgencia" v-model="novoComentario.urgencia" required />
      </p>
      <p>
        <label id="trabalhoLabel" for="trabalho">Trabalho: </label>
        <select id="trabalho" v-model="novoComentario.trabalho.id" required>
          <option v-for="trabalho in trabalhos" :key="trabalho.id" :value="trabalho.id">
            {{ trabalho.titulo }}
          </option>
        </select>
        <input type="number" id="trabalhoInput" v-model="novoComentario.trabalho.id" />
      </p>

      <button type="submit">Confirmar</button>
    </form>

    <!-- pesquisar (consulta) -->
    <form @submit.prevent="pesquisar">
      <p>
        <label id="urgenciaPesquisaLabel" for="urgenciaPesquisa">Urgência: </label>
        <input type="number" id="urgenciaPesquisa" v-model="urgenciaPesquisa" required />
      </p>
      <p>
        <label id="tituloPesquisaLabel" for="tituloPesquisa">Título do trabalho: </label>
        <select id="tituloPesquisa" v-model="tituloPesquisa" required>
          <option v-for="trabalho in trabalhos" :key="trabalho.id" :value="trabalho.titulo">
            {{ trabalho.titulo }}
          </option>
        </select>
        <input type="text" id="tituloPesquisaInput" v-model="tituloPesquisa" required />
      </p>

      <button type="submit">Pesquisar</button>
    </form>

    <!-- tabela listagem -->
    <table v-if="comentarios.length > 0">
      <!-- colunas -->
      <thead>
        <tr>
          <th>Id</th>
          <th>Conteúdo</th>
          <th>Título do Trabalho</th>
          <th>Situação</th>
        </tr>
      </thead>

      <!-- linhas -->
      <tbody>
        <tr v-for="comentario in comentarios" :key="comentario.id">
          <th>{{ comentario.id }}</th>
          <th>{{ comentario.conteudo }}</th>
          <th>{{ comentario.trabalho?.titulo }}</th>
          <th>{{ situacao(comentario.dataHoraCriacao, comentario.urgencia) }}</th>
        </tr>
      </tbody>
    </table>
    <p v-else>Nenhum registro foi encontrado para os critérios fornecidos</p>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'
import { onMounted } from 'vue' //para acessar o hook - quando abrir a pagina, ja buscar os dados

interface usuario {
  id: number
  nome?: string
  senha?: string
}

interface trabalho {
  id: number
  titulo?: string
  dataHoraEntrega?: string
  descricao?: string
  nota?: number
  usuario?: usuario
}

interface comentario {
  id?: number
  conteudo: string
  dataHoraCriacao: string
  urgencia: number
  trabalho: trabalho
}

const novoComentario = ref<comentario>({
  conteudo: '',
  dataHoraCriacao: '',
  urgencia: 1,
  trabalho: { id: 1 },
})
const comentarios = ref<comentario[]>([
  {
    id: 1,
    conteudo: 'bla',
    dataHoraCriacao: '2025-11-09T21:38:43.950827',
    urgencia: 1,
    trabalho: { id: 1 },
  },
])
const trabalhos = ref<trabalho[]>([{ id: 1, titulo: 'Trabalho 1' }])
const erro = ref<string>('')
const tituloPesquisa = ref<string>('')
const urgenciaPesquisa = ref<number>(1)

async function cadastrar() {
  try {
    erro.value = ''
    await axios.post('comentario', novoComentario.value)
    atualizar()
    novoComentario.value.conteudo = ''
    novoComentario.value.dataHoraCriacao = ''
    novoComentario.value.urgencia = 1
    novoComentario.value.trabalho.id = 1
  } catch (e) {
    erro.value = (e as Error).message
  }
}

async function atualizar() {
  comentarios.value = (await axios.get('comentario')).data
}

async function pesquisar() {
  comentarios.value = (
    await axios.get('comentario/busca', {
      params: {
        trabalho: tituloPesquisa.value,
        urgencia: urgenciaPesquisa.value,
      },
    })
  ).data
}

async function buscarTrabalhos() {
  trabalhos.value = (await axios.get('trabalho')).data
}

function situacao(data: string, urgencia: number) {
  const diferencaEmMilissegundos = Date.now() - Date.parse(data)
  const milissegundosPorDia = 1000 * 60 * 60 * 24
  const dias = Math.round(Math.abs(diferencaEmMilissegundos) / milissegundosPorDia)

  if (dias > 10) {
    return 'Atenção'
  } else if (urgencia === 3) {
    return 'Atenção'
  }
  else {
    return 'Ok'
  }
}

onMounted(() => {
  atualizar()
  buscarTrabalhos()
})
</script>
