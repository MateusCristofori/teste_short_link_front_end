<template>
  <form
    @submit.prevent="submitLink"
    method="post"
    class="flex flex-col items-center justify-center min-h-screen bg-gray-100"
  >
    <div class="w-full max-w-md p-4 bg-white rounded shadow">
      <label class="block text-gray-700 text-sm font-bold mb-2"
        >Formulário para encurtamento de Link's</label
      >
      <input
        type="text"
        id="original_url"
        placeholder="Cole seu Link aqui"
        v-model="original_url"
        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
        ref="input_original_url"
      />
      <input
        type="submit"
        value="Encurtar"
        class="mt-4 px-4 py-2 text-white bg-indigo-600 rounded hover:bg-indigo-500"
      />
    </div>
    <div class="w-full max-w-md p-4 mt-4 bg-white rounded shadow">
      <input
        id="shorted_link"
        v-model="shorted_link_value"
        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
        placeholder="Link Encurtado"
      />
      <button
        type="submit"
        @click="redirect"
        :disabled="buttonIsDisabled"
        class="mt-4 px-4 py-2 text-white bg-indigo-600 rounded hover:bg-indigo-500"
      >
        Redirecionar
      </button>
    </div>
  </form>
</template>

<script>
import axios from 'axios'
import { toast } from 'vue3-toastify'
import 'vue3-toastify/dist/index.css'

export default {
  data() {
    return {
      original_url: '',
      shorted_link_value: '',
      buttonIsDisabled: true
    }
  },
  methods: {
    submitLink() {
      if (this.original_url === '') {
        this.$refs.input_original_url.focus()
        return toast.error('Uma URL precisa ser informada!')
      }
      if (!this.validarURL(this.original_url)) {
        this.original_url = ''
        this.$refs.input_original_url.focus()
        return toast.error('A URL precisa ser válida')
      }
      axios
        .post('http://localhost:8080/api', {
          original_url: this.original_url
        })
        .then((res) => {
          toast.success('Link Encurtado com sucesso')
          this.buttonIsDisabled = false
          this.shorted_link_value = res.data['shorted_link']
        })
    },
    redirect() {
      event.preventDefault()
      axios.get(`http://localhost:8080/api${this.shorted_link_value}`).then((res) => {
        window.open(res.data['original_url'], '_blank')
      })
    },
    validarURL(url) {
      try {
        new URL(url)
        return true
      } catch (error) {
        return false
      }
    }
  }
}
</script>
