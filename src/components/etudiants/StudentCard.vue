<script setup>
// Fiche détaillée d'un étudiant (TP3 – exercice 1 : props)
const props = defineProps({
  nom: String,
  niveau: String,
  specialite: String,
  moyenne: Number
})

const emit = defineEmits(['fermer'])
</script>

<template>
  <div class="card fiche">
    <div class="fiche-entete">
      <h3 class="card-titre">🪪 Fiche étudiant</h3>
      <button class="fermer" title="Fermer" @click="emit('fermer')">✕</button>
    </div>

    <div class="identite">
      <span class="avatar">{{ props.nom.charAt(0) }}</span>
      <div>
        <h2>{{ props.nom }}</h2>
        <p>{{ props.specialite }}</p>
      </div>
    </div>

    <ul class="infos">
      <li><span>Niveau</span><strong>{{ props.niveau }}</strong></li>
      <li><span>Moyenne</span><strong>{{ props.moyenne }} / 20</strong></li>
      <li>
        <span>Statut</span>
        <span v-if="props.moyenne >= 10" class="badge badge-succes">Admis</span>
        <span v-else class="badge badge-danger">Ajourné</span>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.fiche {
  border-top: 4px solid var(--primary);
}

.fiche-entete {
  display: flex;
  justify-content: space-between;
}

.fermer {
  border: none;
  background: transparent;
  color: var(--muted);
  font-size: 1rem;
  cursor: pointer;
}

.identite {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-bottom: 16px;
}

.identite h2 {
  font-size: 1.15rem;
}

.identite p {
  color: var(--muted);
}

.avatar {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 54px;
  height: 54px;
  border-radius: 50%;
  background: var(--primary);
  color: #ffffff;
  font-size: 1.4rem;
  font-weight: 700;
}

.infos {
  list-style: none;
}

.infos li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  border-top: 1px solid var(--border);
}

.infos li span:first-child {
  color: var(--muted);
}
</style>
