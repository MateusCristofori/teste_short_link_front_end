<template>
  <main class="flex flex-col items-center justify-center min-h-screen bg-gray-100">
    <div class="w-full max-w-6xl p-4 flex flex-wrap justify-start">
      <div v-if="this.allLinks.length === 0" class="w-full text-center text-gray-500">
        <p>Nenhum link foi encontrado!</p>
      </div>
      <div v-else class="w-full flex flex-wrap justify-start">
        <div
          v-for="link in this.allLinks"
          :key="link.id"
          class="w-full sm:w-1/2 lg:w-1/3 xl:w-1/4 2xl:w-1/5 m-2 bg-white rounded shadow p-4"
        >
          <ul>
            <li>
              <div class="font-medium text-indigo-600">
                <strong>Link Original: </strong>
              </div>
              <div class="pl-4 text-gray-700 mb-2 overflow-x-auto">{{ link.original_url }}</div>
              <div class="font-medium text-indigo-600">
                <strong>Link Encurtado: </strong>
              </div>
              <div class="pl-4 text-gray-700">
                <a
                  @click="redirect(link.hash)"
                  :href="link.hash"
                  target="_blank"
                  class="text-green-500 hover:underline"
                  >{{ link.hash }}
                </a>
              </div>
              <div class="font-medium text-indigo-600">
                <strong>Criado em: </strong>
              </div>
              <div class="pl-4 text-gray-700 mb-2 overflow-x-auto">
                {{ new Date(link.created_at).toLocaleString('pt-br') }}
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <RouterLink
      to="/form"
      class="mt-4 px-4 py-2 text-white bg-indigo-600 rounded hover:bg-indigo-500"
      >Encurtar Link</RouterLink
    >
  </main>
</template>

<script>
import axios from 'axios'
import { toast } from 'vue3-toastify'
import 'vue3-toastify/dist/index.css'

export default {
  setup() {
    toast('Seja bem-vindo(a)!');    
  },
  data() {
    return {
      allLinks: []
    }
  },
  created() {
    this.getAllLinksFromAPI()
  },
  methods: {
    getAllLinksFromAPI() {
      axios.get('http://localhost:8080/api').then((res) => {
        console.log(res.data)
        this.allLinks = res.data.data
      })
    },
    redirect(hash) {
      event.preventDefault()
      axios.get(`http://localhost:8080/api${hash}`).then((res) => {
        console.log(res.data)
        window.open(res.data['original_url'], '_blank')
      })
    }
  }
}
</script>
