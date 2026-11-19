<script setup>
import { ref } from 'vue'
import ThemeSelector from './ThemeSelector.vue'

// Prop reçue du parent (App.vue) : le titre de l'application
defineProps({
  titre: String
})

// Variable réactive locale : l'utilisateur est-il connecté ? (TP2)
const connecte = ref(false)
</script>

<template>
  <header class="header">
    <div>
      <h2 class="header-titre">{{ titre }}</h2>
      <p class="header-sous-titre">Plateforme de gestion universitaire</p>
    </div>

    <div class="header-actions">
      <ThemeSelector />

      <div class="utilisateur">
        <!-- v-if / v-else : un seul des deux messages existe dans le DOM -->
        <span v-if="connecte" class="statut statut-on">● Connecté : Admin</span>
        <span v-else class="statut statut-off">● Non connecté</span>

        <button class="btn btn-secondaire" @click="connecte = !connecte">
          {{ connecte ? 'Déconnexion' : 'Connexion' }}
        </button>
      </div>
    </div>
  </header>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 16px;
  padding: 16px 28px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
}

.header-titre {
  font-size: 1.25rem;
  color: var(--primary);
}

.header-sous-titre {
  font-size: 0.85rem;
  color: var(--muted);
}

.header-actions {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 16px;
}

.utilisateur {
  display: flex;
  align-items: center;
  gap: 10px;
}

.statut {
  font-size: 0.85rem;
  font-weight: 600;
}

.statut-on {
  color: var(--success);
}

.statut-off {
  color: var(--muted);
}
</style>
