<template>
  <section class="py-16 bg-gray-50 min-h-[50vh]">
    <div class="container mx-auto px-4">
      <div class="text-center mb-10">
        <h2 class="text-3xl font-bold text-gray-800 mb-4">
          Palavras do Dia
        </h2>
        
        <div class="flex flex-col md:flex-row justify-center items-center gap-4 mb-6">
          <button 
            @click="fetchWords" 
            :disabled="loading"
            class="bg-blue-600 text-white px-6 py-2 rounded-full font-semibold hover:bg-blue-700 transition duration-200 disabled:opacity-50 disabled:cursor-not-allowed shadow-md hover:shadow-lg w-full md:w-auto"
          >
            {{ loading ? 'Carregando...' : 'Gerar Novas Palavras' }}
          </button>
          
          <!-- Filter Input -->
          <div class="relative w-full md:w-64">
            <input 
              v-model="searchQuery" 
              type="text" 
              placeholder="Filtrar palavras..." 
              class="w-full px-4 py-2 rounded-full border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition duration-200 shadow-sm"
              :disabled="loading || words.length === 0"
            />
            <span v-if="searchQuery" @click="searchQuery = ''" class="absolute right-3 top-2.5 text-gray-400 cursor-pointer hover:text-gray-600">
              ✕
            </span>
          </div>
        </div>
      </div>

      <!-- Loading State -->
      <div v-if="loading" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 max-w-6xl mx-auto">
        <div v-for="i in 3" :key="i" class="bg-white p-6 rounded-lg shadow-md animate-pulse border border-gray-100">
          <div class="h-8 bg-gray-200 rounded w-1/2 mb-4"></div>
          <div class="h-4 bg-gray-200 rounded w-full mb-2"></div>
          <div class="h-4 bg-gray-200 rounded w-5/6 mb-6"></div>
          <div class="h-20 bg-blue-50 rounded w-full"></div>
        </div>
      </div>

      <!-- Error State -->
      <div v-else-if="error" class="text-center max-w-lg mx-auto p-8 bg-white rounded-lg shadow-sm border border-red-100">
        <div class="text-red-500 text-5xl mb-4">😕</div>
        <h3 class="text-xl font-bold text-gray-800 mb-2">Ops! Algo deu errado.</h3>
        <p class="text-gray-600 mb-6">{{ error }}</p>
        <button 
          @click="fetchWords"
          class="bg-white border-2 border-red-500 text-red-500 px-6 py-2 rounded-full font-semibold hover:bg-red-50 transition duration-200"
        >
          Tentar Novamente
        </button>
      </div>

      <!-- Content List -->
      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 max-w-6xl mx-auto">
        <div 
          v-for="(item, index) in filteredWords" 
          :key="index"
          class="bg-white rounded-lg shadow-md hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 overflow-hidden border border-gray-100 flex flex-col relative group"
        >
          <!-- Copy Button -->
          <button 
            @click="copyToClipboard(item.word)" 
            class="absolute top-4 right-4 text-gray-400 hover:text-blue-600 bg-white rounded-full p-2 shadow-sm opacity-0 group-hover:opacity-100 transition-opacity duration-200 focus:opacity-100"
            title="Copiar palavra"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z" />
            </svg>
          </button>

          <div class="p-6 flex-grow">
            <h3 class="text-2xl font-bold text-blue-600 mb-3 pr-8">{{ item.word }}</h3>
            <p class="text-gray-600 mb-6 leading-relaxed">{{ item.description }}</p>
            
            <div class="bg-blue-50 p-4 rounded-md border-l-4 border-blue-400">
              <span class="text-xs font-bold text-blue-500 uppercase tracking-wide block mb-1">Exemplo de Uso</span>
              <p class="text-gray-800 italic">"{{ item.useCase }}"</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Empty State / No Results -->
      <div v-if="!loading && !error && words.length > 0 && filteredWords.length === 0" class="text-center text-gray-500 py-10">
        Nenhuma palavra encontrada para "{{ searchQuery }}".
      </div>
      
      <div v-if="!loading && !error && words.length === 0" class="text-center text-gray-500 py-10">
        Nenhuma palavra para exibir. Clique em "Gerar" para começar.
      </div>

    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const words = ref([])
const loading = ref(false)
const error = ref(null)
const searchQuery = ref('')
const STORAGE_KEY = 'douglas_words_last_result'

const filteredWords = computed(() => {
  if (!searchQuery.value) return words.value
  const query = searchQuery.value.toLowerCase()
  return words.value.filter(item => 
    item.word.toLowerCase().includes(query) ||
    item.description.toLowerCase().includes(query) ||
    item.useCase.toLowerCase().includes(query)
  )
})

const copyToClipboard = async (text) => {
  try {
    await navigator.clipboard.writeText(text)
    // Pequeno feedback visual poderia ser adicionado aqui (toast/alert), 
    // mas seguindo "MÍNIMO necessário", deixaremos sem lib de toast.
    alert(`Palavra "${text}" copiada!`)
  } catch (err) {
    console.error('Falha ao copiar:', err)
  }
}

const saveToLocalStorage = (data) => {
  try {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(data))
  } catch (e) {
    console.error('Erro ao salvar no localStorage', e)
  }
}

const loadFromLocalStorage = () => {
  try {
    const data = localStorage.getItem(STORAGE_KEY)
    if (data) {
      words.value = JSON.parse(data)
      return true
    }
  } catch (e) {
    console.error('Erro ao ler do localStorage', e)
  }
  return false
}

const fetchWords = async () => {
  loading.value = true
  error.value = null
  searchQuery.value = '' // Limpa busca ao recarregar
  
  try {
    const response = await fetch('https://fiap-bff-doug.onrender.com/ask')
    if (!response.ok) throw new Error('Falha ao comunicar com o servidor.')
    
    const data = await response.json()
    words.value = data
    saveToLocalStorage(data)
  } catch (err) {
    console.error(err)
    error.value = 'Ocorreu um erro ao buscar as palavras. Por favor, tente novamente.'
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  // Tenta carregar do cache primeiro, se não tiver, busca da API
  const hasCachedData = loadFromLocalStorage()
  if (!hasCachedData) {
    fetchWords()
  }
})
</script>
