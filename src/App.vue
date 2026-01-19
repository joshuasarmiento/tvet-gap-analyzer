<script setup lang="ts">
import { ref, onMounted, watch, computed } from 'vue'

const stats = ref<any[]>([])
const selectedIndex = ref(0)
const loading = ref(false)
const showModal = ref(true)

const options = [
  "Graduates (Region)", "Graduates (Province)", "Graduates (Sector)",
  "Certified (Region)", "Certified (Province)", "Certified (Sector)"
]

const loadData = async () => {
  loading.value = true
  const res = await fetch(`http://localhost:3000/api/analyze?file=${selectedIndex.value}`)
  stats.value = await res.json()
  loading.value = false
}

const getScoreColor = (supply: number, demand: number) => {
  const ratio = supply / demand;
  if (ratio < 0.4) return 'bg-emerald-500'; // High demand, low supply (Good for students)
  if (ratio < 0.7) return 'bg-amber-500';
  return 'bg-slate-400';
}

onMounted(loadData)
watch(selectedIndex, loadData)
</script>

<template>
  <div class="p-8 max-w-5xl mx-auto font-sans text-slate-900">
    <div v-if="showModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-900/70 backdrop-blur-md">
      <div class="bg-white rounded-3xl p-8 max-w-lg w-full shadow-2xl border border-slate-100 relative overflow-y-auto max-h-[90vh]">
        <button @click="showModal = false" class="absolute top-6 right-6 text-slate-400 hover:text-slate-600">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>
        </button>
        
        <div class="mb-6">
          <div class="w-12 h-12 bg-blue-600 text-white rounded-2xl flex items-center justify-center mb-4 shadow-lg shadow-blue-200">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"></path><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"></path></svg>
          </div>
          <h2 class="text-2xl font-black tracking-tight">Intelligence Sources</h2>
          <p class="text-slate-500 mt-2">This dashboard synthesizes two primary data streams to identify employment gaps.</p>
        </div>

        <div class="space-y-6">
          <div class="bg-slate-50 p-4 rounded-2xl border border-slate-100">
            <div class="flex items-center gap-2 mb-2">
              <span class="text-[10px] bg-blue-100 text-blue-700 font-black px-2 py-0.5 rounded-full uppercase">Primary Supply</span>
            </div>
            <h4 class="font-bold text-slate-800">TESDA TVET Statistics (2024)</h4>
            <p class="text-sm text-slate-600 mb-3">Real-time enrollment, graduation, and certification data across all Philippine regions.</p>
            <a href="https://www.tesda.gov.ph/About/TESDA/28" target="_blank" class="text-xs font-bold text-blue-600 hover:underline inline-flex items-center gap-1">
              View Official Datasets 
              <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"></path><polyline points="15 3 21 3 21 9"></polyline><line x1="10" y1="14" x2="21" y2="3"></line></svg>
            </a>
          </div>

          <div class="bg-slate-50 p-4 rounded-2xl border border-slate-100">
            <div class="flex items-center gap-2 mb-2">
              <span class="text-[10px] bg-emerald-100 text-emerald-700 font-black px-2 py-0.5 rounded-full uppercase">Demand Logic</span>
            </div>
            <h4 class="font-bold text-slate-800">DOLE Labor Market Trends</h4>
            <p class="text-sm text-slate-600">Calculated using 2024 multipliers for high-priority sectors like Construction (2.4x) and ICT (2.8x) based on the "Build Better More" infra initiative.</p>
          </div>
        </div>

        <div class="mt-8 pt-6 border-t border-slate-100">
          <button @click="showModal = false" 
            class="w-full py-4 bg-slate-900 hover:bg-black text-white font-bold rounded-2xl transition-all shadow-xl shadow-slate-200">
            Enter Dashboard
          </button>
        </div>
      </div>
    </div>

    <header class="mb-12 border-b pb-8">
      <div class="flex justify-between items-start">
        <div>
          <h1 class="text-4xl font-black tracking-tighter mb-2">TVET-Employment Gap Analyzer</h1>
          <p class="text-slate-500">Mapping TESDA Certifications against DOLE Industry Demand (2024)</p>
        </div>
        <button @click="showModal = true" class="flex items-center gap-2 px-3 py-2 bg-slate-100 hover:bg-blue-100 text-slate-600 hover:text-blue-700 rounded-xl transition-all font-bold text-xs">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"></circle><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"></path><line x1="12" y1="17" x2="12.01" y2="17"></line></svg>
          Data Sources
        </button>
      </div>
      
      <div class="mt-6 flex flex-wrap gap-2">
        <button v-for="(label, i) in options" :key="i" @click="selectedIndex = i"
          :class="['px-5 py-2.5 rounded-full text-xs font-bold transition-all border', 
          selectedIndex === i ? 'bg-blue-600 text-white border-blue-600 shadow-lg shadow-blue-100' : 'bg-white text-slate-600 border-slate-200 hover:border-blue-400']">
          {{ label }}
        </button>
      </div>
    </header>

    <div v-if="loading" class="text-center py-20">
      <div class="inline-block animate-spin rounded-full h-8 w-8 border-4 border-blue-600 border-t-transparent mb-4"></div>
      <p class="text-slate-400 font-bold tracking-widest uppercase text-xs">Parsing Datasets...</p>
    </div>

    <div v-else class="grid gap-4">
      <div v-for="item in stats" :key="item.label" 
        class="group bg-white border border-slate-200 p-6 rounded-2xl hover:border-blue-500 hover:shadow-xl hover:shadow-slate-100 transition-all flex items-center justify-between">
        <div>
          <h3 class="text-lg font-bold group-hover:text-blue-600">{{ item.label }}</h3>
          <div class="flex gap-4 mt-2 text-sm">
            <span class="text-slate-400">Graduates: <b class="text-slate-700">{{ item.supply.toLocaleString() }}</b></span>
            <span class="text-slate-400">Est. Vacancies: <b class="text-slate-700">{{ item.demand.toLocaleString() }}</b></span>
          </div>
        </div>

        <div class="text-right">
          <p class="text-[10px] font-black uppercase text-slate-400 mb-1">Employment Potential</p>
          <div class="flex items-center gap-3">
            <span :class="['text-[10px] font-black px-2 py-1 rounded text-white tracking-wider', getScoreColor(item.supply, item.demand)]">
              {{ item.gap > 5000 ? 'HIGH VACANCY' : 'COMPETITIVE' }}
            </span>
            <span class="text-2xl font-black text-blue-600">+{{ item.gap.toLocaleString() }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>