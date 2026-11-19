<script setup>
import { ref, computed, provide } from 'vue'

// Composants de mise en page
import Header from './components/layout/Header.vue'
import Footer from './components/layout/Footer.vue'
import Sidebar from './components/layout/Sidebar.vue'

// Vues
import Dashboard from './components/dashboard/Dashboard.vue'

// Module étudiants
import StudentForm from './components/etudiants/StudentForm.vue'
import StudentList from './components/etudiants/StudentList.vue'
import StudentStats from './components/etudiants/StudentStats.vue'
import StudentCard from './components/etudiants/StudentCard.vue'

// ---------- Variables réactives (TP2) ----------
const titre = ref('StudentHub')

// Vue affichée : 'dashboard' ou 'etudiants' (sans Vue Router pour l'instant)
const vueActive = ref('dashboard')

// Thème partagé avec provide/inject (TP3 – exercice 4)
const theme = ref('clair')
provide('theme', theme)

// Le tableau des étudiants est conservé dans App.vue (TP3 – exercice 5)
const etudiants = ref([
  { id: 1, nom: 'Ahmed Ben Ali', specialite: 'Génie Logiciel', niveau: 'L3', moyenne: 14.5 },
  { id: 2, nom: 'Sarra Trabelsi', specialite: 'Informatique', niveau: 'L2', moyenne: 16.25 },
  { id: 3, nom: 'Mohamed Jaziri', specialite: 'Réseaux', niveau: 'L3', moyenne: 9.5 },
  { id: 4, nom: 'Ines Mansouri', specialite: 'Multimédia', niveau: 'L1', moyenne: 12 },
  { id: 5, nom: 'Youssef Hammami', specialite: 'Génie Logiciel', niveau: 'M1', moyenne: 8.75 }
])

// Étudiant affiché dans la fiche détaillée (null = aucune fiche)
const etudiantSelectionne = ref(null)

// ---------- Statistiques calculées (computed) ----------
const totalEtudiants = computed(() => etudiants.value.length)

const nombreAdmis = computed(() => etudiants.value.filter((e) => e.moyenne >= 10).length)

const moyenneGenerale = computed(() => {
  if (etudiants.value.length === 0) return 0
  const somme = etudiants.value.reduce((total, e) => total + e.moyenne, 0)
  return (somme / etudiants.value.length).toFixed(2)
})

// On fournit les statistiques aux composants descendants
provide('totalEtudiants', totalEtudiants)
provide('nombreAdmis', nombreAdmis)
provide('moyenneGenerale', moyenneGenerale)

// ---------- Fonctions appelées par les événements (emit) ----------
const changerVue = (vue) => {
  vueActive.value = vue
}

const ajouterEtudiant = (etudiant) => {
  // Date.now() donne un nombre unique utilisé comme identifiant
  etudiants.value.push({ id: Date.now(), ...etudiant })
}

const supprimerEtudiant = (id) => {
  etudiants.value = etudiants.value.filter((e) => e.id !== id)
  if (etudiantSelectionne.value && etudiantSelectionne.value.id === id) {
    etudiantSelectionne.value = null
  }
}

const selectionnerEtudiant = (etudiant) => {
  etudiantSelectionne.value = etudiant
}
</script>

<template>
  <!-- :class="theme" ajoute la classe "clair" ou "sombre" -->
  <div class="app" :class="theme">
    <Sidebar :vue-active="vueActive" @changer-vue="changerVue" />

    <div class="contenu">
      <Header :titre="titre" />

      <main class="main">
        <!-- Vue 1 : Tableau de bord -->
        <Dashboard
          v-if="vueActive === 'dashboard'"
          :etudiants="etudiants"
          @changer-vue="changerVue"
        />

        <!-- Vue 2 : Gestion des étudiants -->
        <section v-else-if="vueActive === 'etudiants'">
          <h1 class="page-titre">Gestion des étudiants</h1>
          <p class="page-sous-titre">Ajoutez, recherchez et consultez les étudiants inscrits.</p>

          <StudentStats />

          <div class="grille-etudiants">
            <div class="colonne">
              <StudentForm @ajouter-etudiant="ajouterEtudiant" />

              <StudentCard
                v-if="etudiantSelectionne"
                :nom="etudiantSelectionne.nom"
                :niveau="etudiantSelectionne.niveau"
                :specialite="etudiantSelectionne.specialite"
                :moyenne="etudiantSelectionne.moyenne"
                @fermer="etudiantSelectionne = null"
              />
            </div>

            <StudentList
              :etudiants="etudiants"
              @supprimer-etudiant="supprimerEtudiant"
              @selectionner-etudiant="selectionnerEtudiant"
            />
          </div>
        </section>
      </main>

      <Footer />
    </div>
  </div>
</template>

<style scoped>
.app {
  display: grid;
  grid-template-columns: 250px 1fr;
  min-height: 100vh;
  background: var(--bg);
  color: var(--text);
}

.contenu {
  display: flex;
  flex-direction: column;
  min-width: 0;
}

.main {
  flex: 1;
  padding: 28px;
}

.grille-etudiants {
  display: grid;
  grid-template-columns: 340px 1fr;
  gap: 20px;
  align-items: start;
}

.colonne {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* Tablette */
@media (max-width: 1100px) {
  .grille-etudiants {
    grid-template-columns: 1fr;
  }
}

/* Tablette et mobile : la sidebar passe en haut */
@media (max-width: 900px) {
  .app {
    grid-template-columns: 1fr;
  }
}

/* Mobile */
@media (max-width: 600px) {
  .main {
    padding: 16px;
  }
}
</style>
