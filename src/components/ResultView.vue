<template>
  <div class="space-y-8">
    <div class="rounded-3xl bg-white dark:bg-zinc-900 shadow-soft border border-zinc-200/70 dark:border-white/10 p-10 md:p-14 text-center">
      <div class="mx-auto h-16 w-16 rounded-full flex items-center justify-center" :class="tone.bg">
        <svg v-if="tone.kind==='low'" viewBox="0 0 24 24" class="h-8 w-8" :class="tone.fg"><path fill="currentColor" d="M9.55 17L5 12.45l1.4-1.4l3.15 3.15L17.6 6.65L19 8.05z"/></svg>
        <svg v-else viewBox="0 0 24 24" class="h-8 w-8" :class="tone.fg"><path fill="currentColor" d="M12 2a10 10 0 1 0 10 10A10.011 10.011 0 0 0 12 2m1 15h-2v-2h2Zm0-4h-2V7h2Z"/></svg>
      </div>
      <div class="mt-6 text-5xl md:text-6xl font-semibold tracking-tight">{{ formatted }}</div>
      <div class="mt-2 text-lg" :class="tone.text">{{ tone.label }}</div>
      <div class="mt-8 h-px bg-zinc-200 dark:bg-white/10"></div>
      <p class="mt-8 text-zinc-600 dark:text-zinc-300">Indicadores simulados com base no envio. Use apenas como demonstração de interface.</p>
      <div class="mt-10 flex items-center justify-center gap-3">
        <button @click="onBack" class="px-5 py-2.5 rounded-xl bg-zinc-100 dark:bg-zinc-800">Novo exame</button>
        <a v-if="filename" class="px-5 py-2.5 rounded-xl bg-zinc-900 text-white dark:bg-white dark:text-zinc-900" :download="filename" :href="preview">Baixar imagem</a>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({ value: Number, filename: String, onBack: Function })
const preview = computed(() => '')
const formatted = computed(() => `${props.value.toLocaleString('pt-BR', { maximumFractionDigits: 1 })}%`)
const tone = computed(() => {
  const v = props.value
  if (v < 33) return { kind: 'low', label: 'Baixo Risco', text: 'text-emerald-600', bg: 'bg-emerald-50 dark:bg-emerald-500/10', fg: 'text-emerald-600 dark:text-emerald-400' }
  if (v < 66) return { kind: 'mid', label: 'Atenção Moderada', text: 'text-amber-600', bg: 'bg-amber-50 dark:bg-amber-500/10', fg: 'text-amber-600 dark:text-amber-400' }
  return { kind: 'high', label: 'Alto Risco', text: 'text-red-600', bg: 'bg-red-50 dark:bg-red-500/10', fg: 'text-red-600 dark:text-red-400' }
})
const onBack = () => props.onBack && props.onBack()
</script>
