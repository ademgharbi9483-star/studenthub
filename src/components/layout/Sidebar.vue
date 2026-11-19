<script setup>
// Prop : la vue actuellement affichée (envoyée par App.vue)
defineProps({
  vueActive: String
})

// Événement envoyé au parent quand on clique sur un lien
const emit = defineEmits(['changer-vue'])

// Modules disponibles
const liens = [
  { id: 'dashboard', label: 'Tableau de bord', icone: '📊' },
  { id: 'etudiants', label: 'Étudiants', icone: '🎓' }
]

// Modules prévus pour les prochains TPs
const modulesAVenir = [
  { id: 'cours', label: 'Cours', icone: '📚' },
  { id: 'notes', label: 'Notes', icone: '📝' },
  { id: 'absences', label: 'Absences', icone: '📅' },
  { id: 'emploi', label: 'Emploi du temps', icone: '🕒' },
  { id: 'annonces', label: 'Annonces', icone: '📢' }
]
</script>

<template>
  <aside class="sidebar">
    <div class="logo">
      <span class="logo-icone">🎓</span>
      <span class="logo-texte">StudentHub</span>
    </div>

    <nav class="menu">
      <p class="menu-section">Menu</p>
      <button
        v-for="lien in liens"
        :key="lien.id"
        class="menu-lien"
        :class="{ actif: lien.id === vueActive }"
        @click="emit('changer-vue', lien.id)"
      >
        <span>{{ lien.icone }}</span>
        <span>{{ lien.label }}</span>
      </button>

      <p class="menu-section">Bientôt</p>
      <div v-for="module in modulesAVenir" :key="module.id" class="menu-lien desactive">
        <span>{{ module.icone }}</span>
        <span>{{ module.label }}</span>
      </div>
    </nav>
  </aside>
</template>

<style scoped>
.sidebar {
  background: var(--sidebar-bg);
  color: var(--sidebar-text);
  padding: 20px 14px;
  min-height: 100vh;
  position: sticky;
  top: 0;
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 0 10px 24px;
}

.logo-icone {
  font-size: 1.6rem;
}

.logo-texte {
  font-size: 1.25rem;
  font-weight: 700;
  color: #ffffff;
}

.menu {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.menu-section {
  font-size: 0.72rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: #64748b;
  padding: 14px 10px 6px;
}

.menu-lien {
  display: flex;
  align-items: center;
  gap: 12px;
  width: 100%;
  padding: 10px 12px;
  border: none;
  border-radius: 8px;
  background: transparent;
  color: var(--sidebar-text);
  font-size: 0.95rem;
  text-align: left;
  cursor: pointer;
}

.menu-lien:hover {
  background: rgba(255, 255, 255, 0.06);
}

.menu-lien.actif {
  background: var(--primary);
  color: #ffffff;
  font-weight: 600;
}

.menu-lien.desactive {
  opacity: 0.45;
  cursor: default;
}

.menu-lien.desactive:hover {
  background: transparent;
}

/* Tablette et mobile : la sidebar devient une barre horizontale en haut */
@media (max-width: 900px) {
  .sidebar {
    min-height: auto;
    position: static;
    padding: 12px 16px;
  }

  .logo {
    padding: 0 0 10px;
  }

  .menu {
    flex-direction: row;
    overflow-x: auto;
  }

  .menu-section,
  .menu-lien.desactive {
    display: none;
  }

  .menu-lien {
    width: auto;
    white-space: nowrap;
  }
}
</style>
