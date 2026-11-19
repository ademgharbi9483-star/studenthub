<script setup>
// Prop : un seul étudiant, envoyé par StudentList.vue
const props = defineProps({
  etudiant: Object
})

// Événements envoyés au parent (StudentList.vue)
const emit = defineEmits(['selectionner', 'supprimer'])

const confirmerSuppression = () => {
  if (confirm(`Supprimer l'étudiant ${props.etudiant.nom} ?`)) {
    emit('supprimer')
  }
}
</script>

<template>
  <tr>
    <td>
      <div class="identite">
        <span class="avatar">{{ etudiant.nom.charAt(0) }}</span>
        <strong>{{ etudiant.nom }}</strong>
      </div>
    </td>
    <td>{{ etudiant.specialite }}</td>
    <td>{{ etudiant.niveau }}</td>
    <td><strong>{{ etudiant.moyenne }}</strong> / 20</td>
    <td>
      <span v-if="etudiant.moyenne >= 10" class="badge badge-succes">Admis</span>
      <span v-else class="badge badge-danger">Ajourné</span>
    </td>
    <td class="actions">
      <button class="btn btn-secondaire" @click="emit('selectionner')">Détails</button>
      <button class="btn btn-danger" @click="confirmerSuppression">Supprimer</button>
    </td>
  </tr>
</template>

<style scoped>
td {
  padding: 12px 14px;
  border-bottom: 1px solid var(--border);
  white-space: nowrap;
}

.identite {
  display: flex;
  align-items: center;
  gap: 10px;
}

.avatar {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 34px;
  height: 34px;
  border-radius: 50%;
  background: var(--primary-soft);
  color: var(--primary);
  font-weight: 700;
}

.actions {
  display: flex;
  gap: 8px;
}

.actions .btn {
  padding: 6px 12px;
  font-size: 0.8rem;
}
</style>
