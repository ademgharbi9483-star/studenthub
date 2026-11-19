<script setup>
import { ref } from 'vue'

// Champs du formulaire liés avec v-model (TP3 – exercice 3)
const nom = ref('')
const specialite = ref('')
const niveau = ref('L1')
const moyenne = ref(null)
const erreur = ref('')

const niveaux = ['L1', 'L2', 'L3', 'M1', 'M2']

// Événement envoyé au parent avec le nouvel étudiant
const emit = defineEmits(['ajouter-etudiant'])

const ajouter = () => {
  // Validation simple
  if (!nom.value.trim() || !specialite.value.trim()) {
    erreur.value = 'Le nom et la spécialité sont obligatoires.'
    return
  }
  if (moyenne.value === null || moyenne.value === '' || moyenne.value < 0 || moyenne.value > 20) {
    erreur.value = 'La moyenne doit être comprise entre 0 et 20.'
    return
  }

  // On envoie un objet étudiant au parent (App.vue)
  emit('ajouter-etudiant', {
    nom: nom.value.trim(),
    specialite: specialite.value.trim(),
    niveau: niveau.value,
    moyenne: moyenne.value
  })

  // On vide le formulaire
  nom.value = ''
  specialite.value = ''
  niveau.value = 'L1'
  moyenne.value = null
  erreur.value = ''
}
</script>

<template>
  <div class="card">
    <h3 class="card-titre">➕ Ajouter un étudiant</h3>

    <!-- @submit.prevent : empêche le rechargement de la page -->
    <form class="formulaire" @submit.prevent="ajouter">
      <label>
        Nom complet
        <input v-model="nom" class="champ" placeholder="Ex : Ahmed Ben Ali">
      </label>

      <label>
        Spécialité
        <input v-model="specialite" class="champ" placeholder="Ex : Génie Logiciel">
      </label>

      <div class="ligne">
        <label>
          Niveau
          <select v-model="niveau" class="champ">
            <option v-for="n in niveaux" :key="n" :value="n">{{ n }}</option>
          </select>
        </label>

        <label>
          Moyenne
          <!-- .number : convertit automatiquement le texte en nombre -->
          <input v-model.number="moyenne" type="number" step="0.25" min="0" max="20" class="champ" placeholder="0 - 20">
        </label>
      </div>

      <p v-if="erreur" class="erreur">{{ erreur }}</p>

      <button type="submit" class="btn btn-primaire">Ajouter</button>
    </form>
  </div>
</template>

<style scoped>
.formulaire {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

label {
  display: flex;
  flex-direction: column;
  gap: 6px;
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--muted);
}

.ligne {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

.erreur {
  background: var(--danger-soft);
  color: var(--danger);
  padding: 8px 12px;
  border-radius: 8px;
  font-size: 0.85rem;
}
</style>
