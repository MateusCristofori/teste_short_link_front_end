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
    <button
      v-if="isLastPage"
      @click="firstPage"
      class="w-72 mb-2 sm:mb-0 px-2 py-2 text-white bg-green-600 rounded hover:bg-green-500"
    >
      Primeira Página
    </button>
    <div class="w-full flex justify-center items-center mt-4 max-w-6xl">
      <button
        @click="previousPage"
        class="mb-2 sm:mb-0 px-2 py-2 text-white bg-indigo-600 rounded hover:bg-indigo-500"
      >
        Página anterior
      </button>
      <RouterLink
        to="/form"
        class="mb-2 sm:mb-0 mr-2 ml-2 px-2 py-2 text-white bg-indigo-600 rounded hover:bg-indigo-500"
      >
        Encurtar Link
      </RouterLink>
      <button
        @click="nextPage"
        class="mb-2 sm:mb-0 px-2 py-2 text-white bg-indigo-600 rounded hover:bg-indigo-500"
        :disabled="isLastPage"
      >
        Próxima página
      </button>
    </div>
  </main>
</template>

<script>
import axios from 'axios'
import 'vue3-toastify/dist/index.css'
import { toast } from 'vue3-toastify'
import { ref } from 'vue'

export default {
  setup() {
    const isLastPage = ref(false)
    const allLinks = ref([])
    const page = ref(1)

    const getAllLinksFromAPI = () => {
      axios.get(`http://localhost:8080/api?page=${page.value}`).then((res) => {
        console.log(res.data)
        if (res.data.data.length === 0) {
          isLastPage.value = true
          return toast.error('Não existe mais registros!')
        }
        allLinks.value = res.data.data
      })
    }

    const redirect = (hash) => {
      event.preventDefault()
      axios.get(`http://localhost:8080/api${hash}`).then((res) => {
        console.log(res.data)
        window.open(res.data['original_url'], '_blank')
      })
    }
    const nextPage = () => {
      event.preventDefault()
      page.value++
      getAllLinksFromAPI()
    }

    const previousPage = () => {
      event.preventDefault()
      page.value--
      getAllLinksFromAPI()
    }

    const firstPage = () => {
      event.preventDefault()
      page.value = 1
      isLastPage.value = false
      getAllLinksFromAPI()
    }

    return {
      allLinks,
      page,
      nextPage,
      redirect,
      previousPage,
      firstPage,
      isLastPage,
      getAllLinksFromAPI
    }
  },
  mounted() {
    this.getAllLinksFromAPI()
  }
}
</script>
