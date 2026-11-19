<script setup>
import { inject } from 'vue'

// On récupère la ref "theme" fournie par App.vue avec provide (TP3 – exercice 4)
const theme = inject('theme')

// Version recommandée par le TP : on modifie theme.value dans une fonction
const choisirClair = () => theme.value = 'clair'
const choisirSombre = () => theme.value = 'sombre'
</script>

<template>
  <div class="theme-selector">
    <button :class="{ actif: theme === 'clair' }" @click="choisirClair">☀️ Clair</button>
    <button :class="{ actif: theme === 'sombre' }" @click="choisirSombre">🌙 Sombre</button>
  </div>
</template>

<style scoped>
.theme-selector {
  display: flex;
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 3px;
}

button {
  border: none;
  background: transparent;
  color: var(--muted);
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 0.85rem;
  cursor: pointer;
}

button.actif {
  background: var(--surface);
  color: var(--text);
  box-shadow: var(--shadow);
  font-weight: 600;
}
</style>
