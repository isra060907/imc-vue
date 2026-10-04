<script setup>
import { computed, ref } from 'vue'

const poids = ref('')
const taille = ref('')
const afficherResultat = ref(false)

const imc = computed(() => {
  const kg = Number(poids.value)
  const metres = Number(taille.value) / 100

  if (!Number.isFinite(kg) || !Number.isFinite(metres) || kg <= 0 || metres <= 0) {
    return null
  }

  return kg / (metres * metres)
})

const categorie = computed(() => {
  if (imc.value === null) return ''

  if (imc.value < 18.5) return 'Insuffisance pondérale'
  if (imc.value < 25) return 'Corpulence normale'
  if (imc.value < 30) return 'Surpoids'

  return 'Obésité'
})

const explication = computed(() => {
  if (imc.value === null) return ''

  if (imc.value < 18.5) {
    return 'Votre IMC est inférieur à la plage habituelle.'
  }

  if (imc.value < 25) {
    return 'Votre IMC se situe dans la plage habituelle.'
  }

  if (imc.value < 30) {
    return 'Votre IMC se situe dans la plage du surpoids.'
  }

  return 'Votre IMC se situe dans la plage de l’obésité.'
})
const couleurCategorie = computed(() => {
  if (imc.value === null) return ''

  if (imc.value < 18.5) return 'bleu'
  if (imc.value < 25) return 'vert'
  if (imc.value < 30) return 'orange'

  return 'rouge'
})
function calculer() {
  afficherResultat.value = imc.value !== null
}

function reinitialiser() {
  poids.value = ''
  taille.value = ''
  afficherResultat.value = false
}
</script>

<template>
  <main class="page">
    <section class="card">

      <div class="icon">♥</div>

      <h1>Calculateur d’IMC</h1>

      <p class="intro">
        Saisissez votre poids et votre taille pour calculer votre indice de masse corporelle.
      </p>

      <form @submit.prevent="calculer">

        <div class="fields">

          <label>
            Poids <span class="unit">en kilogrammes</span>
            <input
              v-model="poids"
              type="number"
              min="1"
              max="500"
              step="any"
              required
              placeholder="Ex. 70"
              @input="afficherResultat = false"
            />
          </label>

          <label>
            Taille <span class="unit">en centimètres</span>
            <input
              v-model="taille"
              type="number"
              min="50"
              max="250"
              step="any"
              required
              placeholder="Ex. 175"
              @input="afficherResultat = false"
            />
          </label>

        </div>

        <div class="buttons">
          <button class="primary" type="submit">
            Calculer mon IMC
          </button>

          <button class="secondary" type="button" @click="reinitialiser">
            Effacer
          </button>
        </div>

      </form>

      <div v-if="afficherResultat" class="result":class="couleurCategorie">
        

        <span class="result-label">Votre IMC</span>

        <strong class="number">
          {{ imc.toFixed(1).replace('.', ',') }}
        </strong>

        <span class="category">
          {{ categorie }}
        </span>

        <p>
          {{ explication }}
        </p>

      </div>

      <p class="note">
        IMC = poids (kg) ÷ taille (m)².
        Cet indicateur concerne les adultes et ne remplace pas un avis médical.
      </p>

    </section>
  </main>
</template>

