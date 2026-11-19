<script setup>
import { ref, computed } from 'vue'
import StudentStats from '../etudiants/StudentStats.vue'

// Prop : la liste des étudiants (envoyée par App.vue)
const props = defineProps({
  etudiants: Array
})

// Événement pour demander à App.vue d'afficher une autre vue
const emit = defineEmits(['changer-vue'])

// Variables réactives (TP2)
const afficherMessage = ref(true)
const afficherAnnonces = ref(true)

const annonces = ref([
  { id: 1, titre: 'Début des examens du semestre 1', date: '05/01/2026', type: 'Examens' },
  { id: 2, titre: 'Date limite de dépôt des projets Vue.js', date: '20/12/2025', type: 'Projet' },
  { id: 3, titre: 'Journée portes ouvertes de la faculté', date: '12/12/2025', type: 'Événement' }
])

// Les 3 derniers étudiants ajoutés (computed : recalculé automatiquement)
const derniersEtudiants = computed(() => props.etudiants.slice(-3).reverse())
</script>

<template>
  <section>
    <!-- Bannière d'accueil : v-if la retire complètement du DOM -->
    <div v-if="afficherMessage" class="banniere">
      <div>
        <h1>Bienvenue sur StudentHub 👋</h1>
        <p>Gérez vos étudiants, suivez leurs résultats et consultez les annonces au même endroit.</p>
      </div>
      <div class="banniere-actions">
        <button class="btn btn-blanc" @click="emit('changer-vue', 'etudiants')">Gérer les étudiants</button>
        <button class="btn-fermer" title="Fermer" @click="afficherMessage = false">✕</button>
      </div>
    </div>

    <StudentStats />

    <div class="grille">
      <!-- Derniers étudiants -->
      <div class="card">
        <h3 class="card-titre">Derniers étudiants ajoutés</h3>
        <p v-if="derniersEtudiants.length === 0" class="vide">Aucun étudiant pour le moment.</p>
        <ul v-else class="liste">
          <li v-for="etudiant in derniersEtudiants" :key="etudiant.id">
            <span class="avatar">{{ etudiant.nom.charAt(0) }}</span>
            <div class="liste-info">
              <strong>{{ etudiant.nom }}</strong>
              <small>{{ etudiant.niveau }} · {{ etudiant.specialite }}</small>
            </div>
            <span v-if="etudiant.moyenne >= 10" class="badge badge-succes">{{ etudiant.moyenne }}</span>
            <span v-else class="badge badge-danger">{{ etudiant.moyenne }}</span>
          </li>
        </ul>
      </div>

      <!-- Annonces : v-show garde l'élément dans le DOM et le cache en CSS -->
      <div class="card">
        <div class="card-entete">
          <h3 class="card-titre">📢 Annonces</h3>
          <button class="btn btn-secondaire" @click="afficherAnnonces = !afficherAnnonces">
            {{ afficherAnnonces ? 'Masquer' : 'Afficher' }}
          </button>
        </div>
        <ul v-show="afficherAnnonces" class="liste">
          <li v-for="annonce in annonces" :key="annonce.id">
            <span class="badge badge-type">{{ annonce.type }}</span>
            <div class="liste-info">
              <strong>{{ annonce.titre }}</strong>
              <small>{{ annonce.date }}</small>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </section>
</template>

<style scoped>
.banniere {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 16px;
  padding: 24px 28px;
  margin-bottom: 24px;
  border-radius: 14px;
  background: linear-gradient(135deg, #2563eb, #7c3aed);
  color: #ffffff;
}

.banniere h1 {
  font-size: 1.5rem;
  margin-bottom: 6px;
}

.banniere p {
  opacity: 0.9;
}

.banniere-actions {
  display: flex;
  align-items: center;
  gap: 10px;
}

.btn-blanc {
  background: #ffffff;
  color: #2563eb;
  white-space: nowrap;
}

.btn-fermer {
  background: rgba(255, 255, 255, 0.2);
  color: #ffffff;
  border: none;
  border-radius: 50%;
  width: 32px;
  height: 32px;
  cursor: pointer;
}

.grille {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.card-entete {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.liste {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.liste li {
  display: flex;
  align-items: center;
  gap: 12px;
  padding-bottom: 12px;
  border-bottom: 1px solid var(--border);
}

.liste li:last-child {
  border-bottom: none;
  padding-bottom: 0;
}

.liste-info {
  display: flex;
  flex-direction: column;
  flex: 1;
}

.liste-info small {
  color: var(--muted);
}

.avatar {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 38px;
  height: 38px;
  border-radius: 50%;
  background: var(--primary-soft);
  color: var(--primary);
  font-weight: 700;
}

.badge-type {
  background: var(--warning-soft);
  color: var(--warning);
  min-width: 82px;
  text-align: center;
}

.vide {
  color: var(--muted);
}

@media (max-width: 1100px) {
  .grille {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 600px) {
  .banniere {
    flex-direction: column-reverse;
  }
}
</style>
