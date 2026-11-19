<script setup>
import { ref, computed } from 'vue'
import StudentItem from './StudentItem.vue'

// Prop : le tableau des étudiants (envoyé par App.vue)
const props = defineProps({
  etudiants: Array
})

// On fait remonter les actions vers App.vue
const emit = defineEmits(['supprimer-etudiant', 'selectionner-etudiant'])

// Recherche liée avec v-model
const recherche = ref('')

// Liste filtrée : recalculée automatiquement quand recherche ou etudiants change
const etudiantsFiltres = computed(() => {
  const texte = recherche.value.toLowerCase()
  return props.etudiants.filter((e) =>
    e.nom.toLowerCase().includes(texte) ||
    e.specialite.toLowerCase().includes(texte) ||
    e.niveau.toLowerCase().includes(texte)
  )
})
</script>

<template>
  <div class="card">
    <div class="entete">
      <h3 class="card-titre">📋 Liste des étudiants</h3>
      <input v-model="recherche" class="champ recherche" placeholder="🔍 Rechercher (nom, spécialité, niveau)…">
    </div>

    <p v-if="etudiants.length === 0" class="vide">Aucun étudiant.</p>
    <p v-else-if="etudiantsFiltres.length === 0" class="vide">Aucun résultat pour « {{ recherche }} ».</p>

    <div v-else class="table-conteneur">
      <table>
        <thead>
          <tr>
            <th>Nom</th>
            <th>Spécialité</th>
            <th>Niveau</th>
            <th>Moyenne</th>
            <th>Statut</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <StudentItem
            v-for="etudiant in etudiantsFiltres"
            :key="etudiant.id"
            :etudiant="etudiant"
            @selectionner="emit('selectionner-etudiant', etudiant)"
            @supprimer="emit('supprimer-etudiant', etudiant.id)"
          />
        </tbody>
      </table>
    </div>

    <p class="compteur">{{ etudiantsFiltres.length }} étudiant(s) affiché(s) sur {{ etudiants.length }}</p>
  </div>
</template>

<style scoped>
.entete {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  flex-wrap: wrap;
  gap: 12px;
}

.recherche {
  max-width: 320px;
}

.table-conteneur {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.92rem;
}

th {
  text-align: left;
  padding: 10px 14px;
  color: var(--muted);
  font-size: 0.78rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  border-bottom: 1px solid var(--border);
  background: var(--bg);
}

.vide {
  color: var(--muted);
  padding: 20px 0;
  text-align: center;
}

.compteur {
  margin-top: 12px;
  color: var(--muted);
  font-size: 0.82rem;
}
</style>
