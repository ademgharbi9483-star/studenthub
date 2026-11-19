<script setup>
// L'année est calculée automatiquement
const annee = new Date().getFullYear()
</script>

<template>
  <footer class="footer">
    <p>© {{ annee }} StudentHub · Projet Framework Vue.js</p>
    <p>Développé avec Vue 3 + Vite</p>
  </footer>
</template>

<style scoped>
.footer {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 8px;
  padding: 16px 28px;
  border-top: 1px solid var(--border);
  color: var(--muted);
  font-size: 0.85rem;
}
</style>
