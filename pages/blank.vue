<!-- Probleme lors du refresh de la page lorsqu'elle est deja chargée -->

<script lang="ts" setup>
  // Import the "capitalize" function from the "str" module
  import { capitalize } from '~/utils/str'

  // Get the "t" function from the "useLang" composition API
  const { t } = useLang()

  // Define the page metadata
  definePageMeta({
    layout: 'page',
  })

  // Update the page head with a capitalized title and a meta description
  useHead(() => ({
    title: capitalize(t('Projet')),
    meta: [
      {
        name: 'description',
        // The meta description is currently commented out
        // content: t('pages.blank.description'),
      },
    ],
  }))

  // Declare the "tests" variable as an array of objects with a specific structure
  const tests: { id: number; title: string; cover: string }[] = []

  // Fetch data from a local API endpoint and store it in the "tests" variable
  const { data: projet } = await useFetch('http://localhost:8055/items/projet')
  const projetData = projet.value

  // If the "projetData" is a reactive proxy, get its raw value
  var rawData = projetData
  if (isProxy(projetData)) {
    rawData = toRaw(projetData)
  }

  // If the "projetData" is null, set the "rawData" variable to an empty object
var rawData = {};
if (projetData) {
  // If the "projetData" is a reactive proxy, get its raw value
  if (isProxy(projetData)) {
    rawData = toRaw(projetData)
  } else {
    rawData = projetData.value
  }
}

  // Assign the "data" property of the "rawData" object to the "tests" variable
  tests.push(...rawData.data)
</script>

<template>
  <PageWrapper>
    <PageHeader>
      <PageTitle :text="('Projet')" class="capitalize" />
    </PageHeader>
    <PageBody>
      <PageSection>
          <div v-for="test in tests" :key="test.id" class="text-6xl uppercase">
            {{ test.title }}
            <img :src="'http://localhost:8055/assets/'+ test.cover" alt="">

            <!-- Probleme il récupère la balise <p> -->
            {{ test.description }}
          </div>
      </PageSection>
    </PageBody>
  </PageWrapper>
</template> 



<!-- Probleme lors du refresh de la page lorsqu'elle est deja chargée j'ai essayer de verifier si la var tests est vide de re fetch les données-->

<!-- <script lang="ts" setup>
  // Import the "capitalize" function from the "str" module
  import { capitalize } from '~/utils/str'

  // Get the "t" function from the "useLang" composition API
  const { t } = useLang()

  // Define the page metadata
  definePageMeta({
    layout: 'page',
  })

  // Update the page head with a capitalized title and a meta description
  useHead(() => ({
    title: capitalize(t('Projet')),
    meta: [
      {
        name: 'description',
        // The meta description is currently commented out
        // content: t('pages.blank.description'),
      },
    ],
  }))

  // Declare the "tests" variable as an array of objects with a specific structure
  const tests: { id: number; title: string; cover: string }[] = []

  // Fetch data from a local API endpoint and store it in the "tests" variable
  const { data: projet } = await useFetch('http://localhost:8055/items/projet')
  const projetData = projet.value

  // If the "projetData" is null, set the "rawData" variable to null
  let rawData = projetData ? projetData.value : null;
  if (isProxy(projetData)) {
    rawData = toRaw(projetData)
  }

  // Assign the "data" property of the "rawData" object to the "tests" variable
  tests.push(...rawData.data)
</script> -->

<!-- <template>
  <PageWrapper>
    <PageHeader>
      <PageTitle :text="('Projet')" class="capitalize" />
    </PageHeader>
    <PageBody>
      <PageSection>
        <div v-if="tests" v-for="test in tests" :key="test.id" class="text-6xl uppercase">
          {{ test.title }}
          <img :src="'http://localhost:8055/assets/'+ test.cover" alt="">
        </div>
      </PageSection>
    </PageBody>
  </PageWrapper>
</template> -->
