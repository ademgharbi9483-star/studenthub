<script setup>
import { inject } from 'vue'

// Les statistiques sont fournies par App.vue avec provide (TP3 – exercice 5)
const totalEtudiants = inject('totalEtudiants')
const nombreAdmis = inject('nombreAdmis')
const moyenneGenerale = inject('moyenneGenerale')
</script>

<template>
  <div class="stats">
    <div class="card stat">
      <span class="stat-icone bleu">🎓</span>
      <div>
        <p class="stat-label">Total étudiants</p>
        <p class="stat-valeur">{{ totalEtudiants }}</p>
      </div>
    </div>

    <div class="card stat">
      <span class="stat-icone vert">✅</span>
      <div>
        <p class="stat-label">Admis</p>
        <p class="stat-valeur">{{ nombreAdmis }}</p>
      </div>
    </div>

    <div class="card stat">
      <span class="stat-icone rouge">⚠️</span>
      <div>
        <p class="stat-label">Ajournés</p>
        <p class="stat-valeur">{{ totalEtudiants - nombreAdmis }}</p>
      </div>
    </div>

    <div class="card stat">
      <span class="stat-icone orange">📈</span>
      <div>
        <p class="stat-label">Moyenne générale</p>
        <p class="stat-valeur">{{ moyenneGenerale }} <small>/ 20</small></p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 16px;
  margin-bottom: 24px;
}

.stat {
  display: flex;
  align-items: center;
  gap: 14px;
}

.stat-icone {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 48px;
  height: 48px;
  border-radius: 12px;
  font-size: 1.4rem;
}

.bleu { background: var(--primary-soft); }
.vert { background: var(--success-soft); }
.rouge { background: var(--danger-soft); }
.orange { background: var(--warning-soft); }

.stat-label {
  color: var(--muted);
  font-size: 0.85rem;
}

.stat-valeur {
  font-size: 1.6rem;
  font-weight: 700;
}

.stat-valeur small {
  font-size: 0.85rem;
  font-weight: 400;
  color: var(--muted);
}
</style>
